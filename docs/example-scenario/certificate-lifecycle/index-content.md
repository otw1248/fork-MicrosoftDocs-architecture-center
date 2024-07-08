In cybersecurity, setting up automatic certificate renewal is important to maintaining a secure and reliable environment. Failure to update or renew certificates in a timely manner exposes systems to vulnerabilities. Potentially vulnerable areas include:

- SSL/TLS certificates that are expired.
- Networks that are subject to potential breaches.
- Sensitive data that's unsecured.
- Services that go down for business-to-business processes.
- Brand reputation loss that compromises the integrity and confidentiality of digital transactions.

Azure Key Vault supports [automatic certificate renewal](/azure/key-vault/certificates/overview-renew-certificate?tabs=azure-portal) issued by an integrated certification authority (CA) such as *DigiCert* or *GlobalSign*. For a nonintegrated CA, a [manual](/azure/key-vault/certificates/overview-renew-certificate?tabs=azure-portal#renew-a-nonintegrated-ca-certificate) approach is required.

This article bridges the gap by providing an automatic renewal process tailored to certificates from nonintegrated CAs. This process seamlessly stores the new certificates in Key Vault, improves efficiency, enhances security, and simplifies deployment by integrating with various Azure resources.

An automatic renewal process reduces human error and minimizes service interruptions. When you automate certificate renewal, it not only accelerates the renewal process but decreases the likelihood of errors that might occur during manual handling. When you use the capabilities of Key Vault and its extensions, you can build an efficient automatic process to optimize operations and reliability.

While automatic certificate renewal is the initial focus, a broader objective is to enhance security across all areas of the process. This effort includes how to implement the principle of least privilege (PoLP) or similar access controls by using Key Vault. It also emphasizes the importance of robust logging and monitoring practices for Key Vault. This article highlights the importance of using Key Vault to fortify your entire certificate management lifecycle and demonstrates that the security benefits aren't limited to storing certificates.

You can use Key Vault and its automatic renewal process to continuously update certificates. Automatic renewal plays an important role in the deployment process and helps Azure services that integrate with Key Vault benefit from up-to-date certificates. This article provides insight into how continuous renewal and accessibility contribute to the overall deployment efficiency and reliability of Azure services.

## Architecture

Here's a brief overview of the underlying architecture that powers this solution.

:::image type="content" source="./media/certlc-arch.svg" alt-text="Diagram of the certificate lifecycle management architecture." border="false":::

*Download a [Visio file](https://arch-center.azureedge.net/certlc-arch.vsdx) of this architecture.*

The Azure environment comprises the following platform as a service (PaaS) resources: a Key Vault dedicated to storing certificates only issued by the same nonintegrated CA, an Azure Event Grid system topic, a storage account queue, and an Azure Automation account that exposes a webhook targeted by Event Grid.

This scenario assumes that an existing public key infrastructure (PKI) is already in place and consists of a Microsoft Enterprise CA joined to a domain in Microsoft Entra ID. Both the PKI and the Active Directory domain can reside on Azure or on-premises, and the servers that must be configured for certificate renewal.

The virtual machines (VMs) with certificates to monitor renewal don't need to be joined to Active Directory or Microsoft Entra ID. The sole requirement is for the CA and the hybrid worker, if located on a different VM from the CA, to be joined to Active Directory.

The following sections provide details of the automatic renewal process.

### Workflow

This image shows the automatic workflow for certificate renewal within the Azure ecosystem.

![Diagram of the automatic workflow for certificate renewal within the Azure ecosystem.](./media/workflow.png)

1. **Key Vault configuration:** The initial phase of the renewal process entails storing the certificate object in the designated Certificates section of the key vault.
  
    While not mandatory, you can set up custom email notifications by tagging the certificate with the recipient's email address. Tagging the certificate ensures timely notifications when the renewal process completes. If multiple recipients are necessary, separate their email addresses by a comma or a semicolon. The tag name for this purpose is *Recipient*, and its value is one or more email addresses of the designated administrators.

    When you use tags instead of [built-in certificate notifications](/azure/key-vault/certificates/overview-renew-certificate?tabs=azure-portal#get-notified-about-certificate-expiration), you can apply notifications to a specific certificate with a designated recipient. Built-in certificate notifications apply indiscriminately to all certificates within the key vault and use the same recipient for all.
  
    You can integrate built-in notifications with the solution but use a different approach. While built-in notifications can only notify about an upcoming certificate expiration, the tags can send notifications when the certificate renews on the internal CA and when it's available in Key Vault.

1. **Key Vault extension configuration:** You must equip the servers that need to use the certificates with the Key Vault extension, a versatile tool compatible with [Windows](/azure/virtual-machines/extensions/key-vault-windows) and [Linux](/azure/virtual-machines/extensions/key-vault-linux) systems. Azure infrastructure as a service (IaaS) servers and on-premises or other cloud servers that integrate through [Azure Arc](/azure/azure-arc/overview) are supported. Configure the Key Vault extension to periodically poll Key Vault for any updated certificates. The polling interval is customizable and flexible so it can align with specific operational requirements.

1. **Event Grid integration:** As a certificate approaches expiration, two Event Grid subscriptions intercept this important lifetime event from the key vault.

1. **Event Grid triggers:** One Event Grid subscription sends certificate renewal information to a storage account queue. The other subscription triggers the launch of a runbook through the configured webhook in the Automation account. If the runbook fails to renew the certificate, or if the CA is unavailable, a scheduled process retries renewing the runbook from that point until the queue clears. This process makes the solution robust.
  
   To enhance the solution's resiliency, set up a [dead-letter location](/azure/event-grid/manage-event-delivery#set-dead-letter-location) mechanism. It manages potential errors that might occur during the messages transit from Event Grid to the subscription targets, the storage queue, and the webhook.

1. **Storage account queue:** The runbook launches within the CA server configured as an Automation hybrid runbook worker. It receives all messages in the storage account queue that contain the name of the expiring certificate and the key vault that hosts the runbook. The following steps occur for each message in the queue.

1. **Certificate renewal:** The script in the runbook connects to Azure to retrieve the certificate's template name that you set up during generation. The template is the configuration component of the certification authority that defines the attributes and purpose of the certificates to be generated.

   After the script interfaces with Key Vault, it initiates a certificate renewal request. This request triggers Key Vault to generate a certificate signing request (CSR) and applies the same template that generated the original certificate. This process ensures that the renewed certificate aligns with the predefined security policies. For more information about security in the authentication and authorization process, see the [Security](#security) section.

   The script downloads the CSR and submits it to the CA.

   The CA generates a new x509 certificate based on the correct template and sends it back to the script. This step ensures that the renewed certificate aligns with the predefined security policies.

1. **Certificate merging and Key Vault update:** The script merges the renewed certificate back into the key vault, finalizing the update process and removing the message from the queue. Throughout the entire process, the private key of the certificate is never extracted from the key vault.

1. **Monitoring and email notification:** All operations that various Azure components run, such as an Automation account, Key Vault, a storage account queue, and Event Grid, are logged within the Azure Monitor Logs workspace to enable monitoring. After the certificate merges into the key vault, the script sends an email message to administrators to notify them of the outcome.

1. **Certificate retrieval:** The Key Vault extension on the server plays an important role during this phase. It automatically downloads the latest version of the certificate from the key vault into the local store of the server that's using the certificate. You can configure multiple servers with the Key Vault extension to retrieve the same certificate (wildcard or with multiple Subject Alternative Name (SAN) certificates) from the key vault.

### Components

The solution uses various components to handle automatic certificate renewal on Azure. The following sections describe each component and its specific purpose.

#### Key Vault extension

The Key Vault extension plays a vital role in automating certificate renewal and must be installed on servers that require the automation. For more information on installation procedures on Windows servers, see [Key Vault extension for Windows](/azure/virtual-machines/extensions/key-vault-windows). For more information on installation steps for Linux servers, see [Key Vault extension for Linux](/azure/virtual-machines/extensions/key-vault-linux). For more information on Azure Arc-enabled servers, see [Key Vault extension for Arc-enabled servers](https://techcommunity.microsoft.com/t5/azure-arc-blog/in-preview-azure-key-vault-extension-for-arc-enabled-servers/ba-p/1888739).

> [!NOTE]
> The following are sample scripts that you can run from Azure Cloud Shell for configuring the Key Vault extension:
>
> - [Key Vault extension for Windows servers](https://github.com/Azure/certlc/blob/main/.scripts/kvextensionWin.ps1)
> - [Key Vault extension for Linux servers](https://github.com/Azure/certlc/blob/main/.scripts/kvextensionLinux.ps1)
> - [Key Vault extension for Azure Arc-enabled Windows servers](https://github.com/Azure/certlc/blob/main/.scripts/kvextensionARCWin.ps1)
> - [Key Vault extension for Azure Arc-enabled Linux servers](https://github.com/Azure/certlc/blob/main/.scripts/kvextensionARCLinux.ps1)

The Key Vault extension configuration parameters include:

- **Key Vault Name:** The key vault that contains the certificate for renewal.
- **Certificate Name:** The name of the certificate to be renewed.
- **Certificate Store, Name, and Location:** The certificate store where the certificate is stored. On Windows servers, the default value for *Name* is `My` and *Location* is `LocalMachine`, which is the personal certificate store of the computer. On Linux servers, you can specify a file system path, assuming that the default value is `AzureKeyVault`, which is the certificate store for Key Vault.
- **linkOnRenewal:** A flag that indicates whether the certificate should be linked to the server on renewal. If set to `true` on Windows machines, it copies the new certificate in the store and links it to the old certificate, which effectively rebinds the certificate. The default value is `false` meaning that an explicit binding is required.
- **pollingIntervalInS:** The polling interval for the Key Vault extension to check for certificate updates. The default value is `3600` seconds (1 hour).
- **authenticationSetting:** The authentication setting for the Key Vault extension. For Azure servers, you can omit this setting, meaning that the system-assigned managed identity of the VM is used against the key vault. For on-premises servers, specifying the setting `msiEndpoint = "http://localhost:40342/metadata/identity"` means the usage of the service principal associated with the computer object created during the Azure Arc onboarding.

> [!NOTE]
> Specify the Key Vault extension parameters only during the initial setup. This ensures they won't undergo any changes throughout the renewal process.

#### Automation account

The Automation account handles the certificate renewal process. You need to configure the account with a runbook by using the [PowerShell script](https://github.com/Azure/certlc/blob/main/.runbook/runbook_v3.ps1).

You also need to create a Hybrid Worker Group. Associate the Hybrid Worker Group with a Windows Server member of the same Active Directory domain of the CA, ideally the CA itself, for launching runbooks.

The runbook must have an associated [webhook](/azure/automation/automation-webhooks) initiated from the hybrid runbook worker. Configure the webhook URL in the event subscription of the Event Grid system topic.

#### Storage account queue

The storage account queue stores the messages that contain the name of the certificate being renewed and the key vault that contains the certificate. Configure the storage account queue in the event subscription of the Event Grid system topic. The queue handles decoupling the script from the certificate expiration notification event. It supports persisting the event within a queue message. This approach helps ensure that the renewal process for certificates is repeated through scheduled jobs even if there are problems that occur during the script's run.

#### Hybrid runbook worker

The hybrid runbook worker plays a vital role in using runbooks. You need to install the hybrid runbook worker with the [Azure Hybrid Worker extension](/azure/automation/extension-based-hybrid-runbook-worker-install) method, which is the supported mode for a new installation. You create it and associate it with a Windows Server member in the same Active Directory domain of the CA, ideally the CA itself.

#### Key Vault

Key Vault is the secure repository for certificates. Under the event section of the key vault, associate the Event Grid system topic with the webhook of the Automation account and a subscription.

#### Event Grid

Event Grid handles event-driven communication within Azure. Configure Event Grid by setting up the system topic and event subscription to monitor relevant events. Relevant events include certificate expiration alerts, triggering actions within the automation workflow, and posting messages in the storage account queue. Configure the Event Grid system topic with the following parameters:

- **Source:** The name of the key vault containing the certificates.
- **Source Type:** The type of the source. For example, the source type for this solution is `Azure Key Vault`.
- **Event Types:** The event type to be monitored. For example, the event type for this solution is `Microsoft.KeyVault.CertificateNearExpiry`. This event triggers when a certificate is near expiration.
- **Subscription for Webhook:**
  - **Subscription Name:** The name of the event subscription.
  - **Endpoint Type:** The type of endpoint to be used. For example, the endpoint type for this solution is `Webhook`.
  - **Endpoint:** The URL of the webhook associated with the Automation account runbook. For more information, see the [Automation account](#automation-account) section.
- **Subscription for StorageQueue:**
  - **Subscription Name:** The name of the event subscription.
  - **Endpoint Type:** The type of endpoint to be used. For example, the endpoint type for this solution is `StorageQueue`.
  - **Endpoint:** The storage account queue.

### Alternatives

This solution uses an Automation account to orchestrate the certificate renewal process and uses hybrid runbook worker to provide the flexibility to integrate with a CA on-premises or in other clouds.

An alternative approach is to use Logic Apps. The main difference between the two approaches is that the Automation account is a platform as a service (PaaS) solution, while Logic Apps is a software as a service (SaaS) solution.

The main advantage of Logic Apps is that it's a fully managed service. You don't need to worry about the underlying infrastructure. Also, Logic Apps can easily integrate with external connectors, expanding the range of notification possibilities, such as engaging with Microsoft Teams or Microsoft 365.

Logic Apps doesn't have a feature similar to hybrid runbook worker, which results in less flexible integration with the CA, so an Automation account is the preferred approach.

## Scenario details

Every organization requires secure and efficient management of their certificate lifecycle. Failing to update a certificate before expiration can lead to service interruptions and incur significant costs for the business.

Enterprises typically operate complex IT infrastructures involving multiple teams responsible for the certificate lifecycle. The manual nature of the certificate renewal process often introduces errors and consumes valuable time.

This solution addresses the challenges by automating certificate renewal issued by Microsoft Certificate Service. The service is widely used for various server applications such as web servers, SQL servers, and for encryption, nonrepudiation, signing purposes, and ensuring timely updates and secure certificate storage within Key Vault. The service’s compatibility with Azure servers and on-premises servers supports flexible deployment.

### Potential use cases

This solution caters to organizations across various industries that:

- Use Microsoft Certificate Service for server certificate generation.
- Require automation in the certificate renewal process to accelerate operations and minimize errors, which helps avoid business loss and service-level agreement (SLA) violations.
- Require secure certificate storage in repositories like Key Vault.

This architecture serves as a foundational deployment approach across application landing zone subscriptions.

## Considerations

These considerations implement the pillars of the Azure Well-Architected Framework, which is a set of guiding tenets that you can use to improve the quality of a workload. For more information, see [Microsoft Azure Well-Architected Framework](/azure/well-architected/).

### Security

Security provides assurances against deliberate attacks and the abuse of your valuable data and systems. For more information, see [Overview of the security pillar](/azure/architecture/framework/security/overview).

Within the key vault system, certificates are securely stored as encrypted secrets, protected by Azure role-based access control (RBAC).

Throughout the certificate renewal process, components that use identities are:

- The system account of the hybrid runbook worker, which operates under the VM's account.
- The Key Vault extension, which uses the managed identity associated with the VM.
- The Automation account, which uses its designated managed identity.

The principle of least privilege is rigorously enforced across all identities engaged in the certificate renewal procedure.

The system account of the hybrid runbook worker server must have the right to enroll certificates on one or more certificate templates that generate new certificates.

On the key vault containing the certificates, the Automation account identity must have the `Key Vault Certificate Officer` role. Additionally, servers requiring certificate access must have `Get` and `List` permissions within the key vault's certificate store.

On the storage account queue, the Automation account identity must have the `Storage Queue Data Contributor`, `Reader and Data Access`, and `Reader` roles.

In scenarios where the Key Vault extension deploys on an Azure VM, the authentication occurs via the managed identity of the VM. However, when deployed on an Azure Arc-enabled server, authentication is handled using a service principal. Both the managed identity and service principal must be assigned the key vault secret user role within the key vault that stores the certificate. You must use a secret role because the certificate is stored in the key vault as a secret.

### Cost optimization

Cost optimization is about looking at ways to reduce unnecessary expenses and improve operational efficiencies. For more information, see [Design review checklist for Cost Optimization](/azure/well-architected/cost-optimization/checklist).

This solution uses Azure PaaS solutions that operate under a pay-as-you-go framework to optimize cost. Expenses depend on the number of certificates that need renewal and the number of servers equipped with the Key Vault extension, which results in low overhead.

Expenses that result from the Key Vault extension and the hybrid runbook worker depend on your installation choices and polling intervals. The cost of Event Grid corresponds to the volume of events generated by Key Vault. At the same time, the cost of the Automation account correlates with the number of runbooks you use.

The cost of Key Vault depends on various factors, including your chosen SKU (Standard or Premium), the quantity of stored certificates, and the frequency of operations conducted on the certificates.

Similar considerations to the configurations described for Key Vault apply equally to the storage account. In this scenario, a standard SKU with locally redundant storage replication suffices for the storage account. Generally, the cost of the storage account queue is minimal.

To estimate the cost of implementing this solution, use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator), inputting the services described in this article.

### Operational excellence

Operational excellence covers the operations processes that deploy an application and keep it running in production. For more information, see [Design review checklist for Operational Excellence](/azure/well-architected/operational-excellence/checklist).

The automatic certificate renewal procedure securely stores certificates by way of standardized processes applicable across all certificates within the key vault.

Integrating with Event Grid triggers supplementary actions, such as notifying Microsoft Teams or Microsoft 365 and streamlining the renewal process. This integration significantly reduces certificate renewal time and mitigates the potential for errors that could lead to business disruptions and violations of SLAs.

Also, seamless integration with Azure Monitor, Microsoft Sentinel, Microsoft Copilot for Security, and Azure Security Center facilitates continuous monitoring of the certificate renewal process. It supports anomaly detection and ensures that robust security measures are maintained.

## Deploy this scenario

Select the following button to deploy the environment described in this article. The deployment takes about two minutes to complete and creates a Key Vault, an Event Grid system topic configured with the two subscriptions, a storage account containing the *certlc* queue, and an Automation account containing the *runbook* and the *webhook* linked to Event Grid.

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fcertlc%2Fmain%2F.armtemplate%2Fmindeploy.json)

Detailed information about the parameters needed for the deployment can be found in the [code sample](/samples/azure/certlc/certlc/) portal.

> [!IMPORTANT]
> > You can deploy a full lab environment to demonstrate the entire automatic certificate renewal workflow. Use the [code sample](/samples/azure/certlc/certlc/) to deploy the following resources:
> > - **Active Directory Domain Services (AD DS)** within a domain controller VM.
> > - **Active Directory Certificate Services (AD CS)** within a CA VM, joined to the domain, configured with a template, *WebServerShort*, for enrolling the certificates to renew.
> > - A **Windows Simple Mail Transfer Protocol (SMTP) server** installed on the same VM of the CA for sending email notifications. MailViewer also installs to verify the email notifications sent.
> > - The **Key Vault extension** installed on the VM of the domain controller for retrieving the renewed certificates from the Key Vault extension.
> >
> > [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fcertlc%2Fmain%2F.armtemplate%2Ffulllabdeploy.json)

## Contributors

*This article is maintained by Microsoft. It was originally written by the following contributors.*

Principal authors:

- [Fabio Masciotra](https://www.linkedin.com/in/fabiomasciotra/) | Principal Consultant
- [Angelo Mazzucchi](https://www.linkedin.com/in/angelo-mazzucchi-a5a94270) | Delivery Architect

*To see non-public LinkedIn profiles, sign in to LinkedIn.*

## Related resources

[Key Vault](/azure/key-vault/general/overview)</BR>
[Key Vault extension for Windows](/azure/virtual-machines/extensions/key-vault-windows?tabs=version3)</BR>
[Key Vault extension for Linux](/azure/virtual-machines/extensions/key-vault-linux)</BR>
[What is Azure Automation?](/azure/automation/overview)</BR>
[Azure Automation hybrid runbook worker](/azure/automation/automation-hybrid-runbook-worker)</BR>
[Azure Event Grid](/azure/event-grid/overview)
