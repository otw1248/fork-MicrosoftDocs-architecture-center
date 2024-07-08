---
title: Apache scenarios on Azure
description: Review a list of architectures and solutions that use Apache open-source solutions.
author: martinekuan
ms.date: 07/26/2022
ms.topic: conceptual
ms.service: architecture-center
ms.subservice: azure-guide
products:
  - azure-kubernetes-service
  - azure-stack-hub
  - azure-hdinsight
categories:
  - ai-machine-learning
  - databases
  - hybrid
  - analytics
  - web
  - iot
  - migration 
  - containers
  - integration 
  - compute
  - storage
ms.custom: fcp
---

# Apache open-source scenarios on Azure

Microsoft is proud to support open-source projects, initiatives, and foundations and contribute to thousands of open-source communities. By using open-source technologies on Azure, you can run applications your way while optimizing your investments. 

This article provides a summary of architectures and solutions that use Azure together with Apache open-source solutions.

*Apache®, Apache Ignite, Ignite, and the flame logo are either registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. No endorsement by The Apache Software Foundation is implied by the use of these marks.*

## Apache Cassandra

|Architecture|Summary|Technology focus|
|--|--|--|
|[Data partitioning guidance](../best-practices/data-partitioning.yml)|View guidance for how to separate data partitions to be managed and accessed separately. Understand horizontal, vertical, and functional partitioning strategies. Cassandra is ideally suited to vertical partitioning.|Databases|
|[High availability in Azure public MEC](../example-scenario/hybrid/multi-access-edge-compute-ha.yml)|Learn how to deploy workloads in active-standby mode to achieve high availability and disaster recovery in Azure public multi-access edge compute. Cassandra can be used to support geo-replication.| Hybrid|
|[N-tier application with Apache Cassandra](../databases/architecture/n-tier-cassandra.yml)|Deploy Linux virtual machines and a virtual network configured for an N-tier architecture with Apache Cassandra.| Databases|
|[Non-relational data and NoSQL](../data-guide/big-data/non-relational-data.yml) |Learn about non-relational databases that store data as key-value pairs, graphs, time series, objects, and other storage models, based on data requirements. Azure Cosmos DB for Apache Cassandra is a recommended Azure service.|Databases|
|[Run Apache Cassandra on Azure VMs](../databases/guide/cassandra.md)|Examine performance considerations for running Apache Cassandra on Azure virtual machines. Use these recommendations as a baseline to test against your workload.| Databases|
|[Stream processing with fully managed open-source data engines](../example-scenario/data/open-source-data-engine-stream-processing.yml)|Stream events by using fully managed Azure data services. Use open-source technologies like Kafka, Kubernetes, Cassandra, PostgreSQL, and Redis components.| Analytics|

## Apache CouchDB

|Architecture|Summary|Technology focus|
|--|--|--|
|[Baseline web application with zone redundancy](../web-apps/app-service/architectures/baseline-zone-redundant.yml) |Use the proven practices in this reference architecture to improve redundancy, scalability and performance in an Azure App Service web application. CouchDB is a recommended document database.|Web

## Apache Hadoop

|Architecture|Summary|Technology focus|
|--|--|--|
|[AI at the edge with Azure Stack Hub](../solution-ideas/articles/ai-at-the-edge.yml)|Bring your trained AI model to the edge with Azure Stack Hub. Integrate it with your applications for low-latency intelligence. Use Hadoop to store data.| AI|
|[AI at the edge with Azure Stack Hub - disconnected](../solution-ideas/articles/ai-at-the-edge-disconnected.yml)|Take advantage of edge AI when disconnected from the internet and move your AI models to the edge with a solution that includes Azure Stack Hub. Use Hadoop to store data.| AI|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. Azure HDInsight Hadoop clusters can be used for batch processing.| Databases|
|[Choose a data transfer technology](../data-guide/scenarios/data-transfer.md)|Learn about Azure data transfer options like Azure Import/Export service, Azure Data Box, Azure Data Factory, and command-line and graphical interface tools. The Hadoop ecosystem provides tools for data transfer.| Databases|
|[Citizen AI with Power Platform](../example-scenario/ai/citizen-ai-power-platform.yml)|Learn how to use Azure Machine Learning and Power Platform to quickly create a machine learning proof of concept and production version. Azure Data Lake, a Hadoop-compatible file system, stores data.| AI|
|[Data considerations for microservices](../microservices/design/data-considerations.yml)|Learn about managing data in a microservices architecture. View an example that uses Azure Data Lake Store, a Hadoop file system. | Microservices|
|[Extend your on-premises big data investments with HDInsight](../solution-ideas/articles/extend-your-on-premises-big-data-investments-with-hdinsight.yml)|Extend your on-premises big data investments to the cloud. Transform your business by using the advanced analytics capabilities of HDInsight. Hadoop is used as a data store.| Analytics|
|[Extract, transform, and load](../data-guide/relational-data/etl.yml)|Learn about extract-transform-load (ETL) and extract-load-transform (ELT) data transformation pipelines and how to use control flows and data flows. Hadoop can be used as destination data store in ELT processes.| Analytics|
|[ETL using HDInsight](../solution-ideas/articles/extract-transform-and-load-using-hdinsight.yml)|ETL big data clusters on demand by using HDInsight, Hadoop MapReduce, and Apache Spark.| Analytics|
|[IoT analyze-and-optimize loops](../example-scenario/iot/analyze-optimize-loop.yml)|Learn about analyze-and-optimize loops, an IoT pattern for generating and applying optimization insights based on an entire business context. Hadoop map-reduce processing can be used to process big data.| IoT|
|[Master data management with Azure and CluedIn](../databases/architecture/cluedin.yml)|Use CluedIn eventual connectivity data integration to blend data from many siloed data sources and prepare it for analytics and business operations. CluedIn can take input data from Hadoop.| Databases|
|[Materialized View pattern](../patterns/materialized-view.yml)|Generate prepopulated views over the data in one or more data stores when the data isn't ideally formatted for your required query operations. Use Hadoop for a big data storage mechanism that supports indexing.| Databases|
|[Predict loan charge-offs with HDInsight Spark](../solution-ideas/articles/loan-chargeoff-prediction-with-azure-hdinsight-spark-clusters.yml)|Use HDInsight and machine learning to predict the likelihood of loans getting charged off. HDInsight supports Hadoop.| Databases|

## Apache HBase

|Architecture|Summary|Technology focus|
|--|--|--|
|[AI at the edge with Azure Stack Hub](../solution-ideas/articles/ai-at-the-edge.yml)|Bring your trained AI model to the edge with Azure Stack Hub. Integrate it with your applications for low-latency intelligence. Use HBase to store data.| AI|
|[AI at the edge with Azure Stack Hub - disconnected](../solution-ideas/articles/ai-at-the-edge-disconnected.yml)|Take advantage of edge AI when disconnected from the internet and move your AI models to the edge with a solution that includes Azure Stack Hub. Use HBase to store data.| AI|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. You can use HBase for data presentation in these scenarios.| Databases|
|[Choose a big data storage technology](../data-guide/technology-choices/data-storage.md)|Compare big data storage technology options in Azure. Includes a discussion of HBase on HDInsight.| Databases|
|[Choose an analytical data store](../data-guide/technology-choices/analytical-data-stores.md)|Learn about using HBase for random access and strong consistency for large amounts of unstructured and semi-structured data.| Analytics|
|[Data partitioning guidance](../best-practices/data-partitioning.yml)|View guidance for separating data partitions so they can be managed and accessed separately. Understand horizontal, vertical, and functional partitioning strategies. HBase is ideally suited to vertical partitioning.| Databases|
|[Non-relational data and NoSQL](../data-guide/big-data/non-relational-data.yml)|Learn about non-relational databases that store data as key-value pairs, graphs, time series, objects, and other storage models, based on data requirements. HBase can be used for columnar and time series data. | Databases|

## Apache Hive

|Architecture|Summary|Technology focus|
|--|--|--|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. You can use Hive for batch processing and data presentation in these scenarios.| Databases|
|[Campaign optimization with HDInsight Spark](/azure/architecture/solution-ideas/articles/optimize-marketing-with-machine-learning)|Build and deploy a machine learning model to maximize the purchase rate of leads that are targeted by a marketing campaign. Hive is used to store recommendations for how and when to contact each lead. | Databases|
|[Choose a batch processing technology](../data-guide/technology-choices/batch-processing.md)|Compare technology choices for big data batch processing in Azure. Learn about the capabilities of Hive.| Analytics|
|[Choose an analytical data store](../data-guide/technology-choices/analytical-data-stores.md)|Evaluate analytical data store options for big data in Azure. Learn about the capabilities of Hive.| Analytics|
|[Extract, transform, and load](../data-guide/relational-data/etl.yml)|Learn about ETL and ELT data transformation pipelines and how to use control flows and data flows. In ELT, you can use Hive to query source data. You can also use it together with Hadoop as a data store.| Databases|
|[ETL using HDInsight](../solution-ideas/articles/extract-transform-and-load-using-hdinsight.yml)|ETL big data clusters on demand by using HDInsight, Hive, and Apache Spark. | Analytics|
|[Loan charge-off prediction with HDInsight Spark clusters](../solution-ideas/articles/loan-chargeoff-prediction-with-azure-hdinsight-spark-clusters.yml)|Use HDInsight and machine learning to predict the likelihood of loans getting charged off. Analytics results are stored in Hive tables.| Analytics|
|[Predictive aircraft engine monitoring](../solution-ideas/articles/aircraft-engine-monitoring-for-predictive-maintenance-in-aerospace.yml)|Learn how to combine real-time aircraft data with analytics to create a solution for predictive aircraft engine monitoring and health. Hive scripts provide aggregations on raw events that are archived by Azure Stream Analytics.| Analytics|
|[Predictive insights with vehicle telematics](../solution-ideas/articles/predictive-insights-with-vehicle-telematics.yml)|Learn how car dealerships, manufacturers, and insurance companies can use Azure to get predictive insights on vehicle health and driving habits. In this solution, Azure Data Factory uses HDInsight to run Hive queries to process and load data.| Analytics|
|[Scale AI and machine learning initiatives in regulated industries](../example-scenario/ai/scale-ai-and-machine-learning-in-regulated-industries.yml)|Learn about scaling Azure AI and machine learning environments that must comply with extensive security policies. Hive is used to store metadata.| AI|

## Apache JMeter

|Architecture|Summary|Technology focus|
|--|--|--|
|[Banking system cloud transformation on Azure](../example-scenario/banking/banking-system-cloud-transformation.yml)|Use simulated and actual applications and existing workloads to monitor the reaction of a solution infrastructure for scalability and performance. A custom JMeter solution is used for load testing.|Migration|
[Patterns and implementations for a banking cloud transformation](../example-scenario/banking/patterns-and-implementations.yml)|Learn about the patterns and implementations used to transform a banking system for the cloud. JMeter is used for load testing.|Migration|
[Scalable cloud applications and SRE](../example-scenario/apps/scalable-apps-performance-modeling-site-reliability.yml)|Build scalable cloud applications by using performance modeling and other principles and practices of site reliability engineering (SRE). JMeter is used for load testing.|Web|

## Apache Kafka
 
|Architecture|Summary|Technology focus|
|--|--|--|
|[Application data protection for AKS workloads on Azure NetApp Files](../example-scenario/file-storage/data-protection-kubernetes-astra-azure-netapp-files.yml)|Deploy Astra Control Service with Azure NetApp Files for data protection, disaster recovery, and mobility for Azure Kubernetes Service (AKS) applications, including Kafka applications.|Containers|
|[Asynchronous messaging options](../guide/technology-choices/messaging.yml)|Learn about asynchronous messaging options in Azure, including support for Kafka clients.|Integration|
|[Automated guided vehicles fleet control](../example-scenario/iot/automated-guided-vehicles-fleet-control.yml)|Learn about an end-to-end approach for an automotive original equipment manufacturer (OEM). Includes several open-source libraries that you can reuse. Back-end services in this architecture can connect to Kafka.|Web|
|[Azure Data Explorer interactive analytics](../solution-ideas/articles/interactive-azure-data-explorer.yml)|Ingest Kafka data into Azure Data Explorer and examine it by using improvised, interactive, fast queries.|Analytics|
|[Azure Data Explorer monitoring](../solution-ideas/articles/monitor-azure-data-explorer.yml)|Use Azure Data Explorer in a hybrid monitoring solution that ingests streamed and batched logs from Kafka and other sources.|Analytics|
|[Banking system cloud transformation on Azure](../example-scenario/banking/banking-system-cloud-transformation.yml)|Use simulated and actual applications and existing workloads to monitor the reaction of a solution infrastructure for scalability and performance. Events from Event Hubs for Kafka feed into the system. |Containers|
|[Choose a stream processing technology](../data-guide/technology-choices/stream-processing.md)|Compare options for real-time message stream processing in Azure, including the Kafka streams API.|Analytics|
|[Claim-Check pattern](../patterns/claim-check.yml)|Examine the Claim-Check pattern, which splits a large message into a claim check and a payload to avoid overwhelming a message bus. Learn about an example that uses Kafka for claim-check generation.|Integration|
|[Data streaming with AKS](../solution-ideas/articles/data-streaming-scenario.yml)|Use AKS to easily ingest and process a real-time data stream with millions of data points collected via sensors. Kafka stores data for analysis.|Containers|
|[Ingestion, ETL, and stream processing pipelines with Azure Databricks](../solution-ideas/articles/ingest-etl-stream-with-adb.yml)|Create ETL pipelines for batch and streaming data with Azure Databricks to simplify data lake ingestion at any scale. Kafka is one option for ingesting data.|Analytics|
|[Integrate Event Hubs with Azure Functions](../serverless/event-hubs-functions/event-hubs-functions.yml)|Learn how to architect, develop, and deploy efficient and scalable code that runs on Azure Functions and responds to Azure Event Hubs events. Learn how events can be persisted in Kafka topics. |Serverless|
|[IoT analytics with Azure Data Explorer](../solution-ideas/articles/iot-azure-data-explorer.yml)|Use Azure Data Explorer for near real-time IoT telemetry analytics on fast-flowing, high-volume streaming data from a variety of data sources, including Kafka.|Analytics|
|[Mainframe and midrange data replication to Azure using Qlik](../example-scenario/mainframe/mainframe-midrange-data-replication-azure-qlik.yml)|Use Qlik Replicate to migrate mainframe and midrange systems to the cloud, or to extend such systems with cloud applications. In this solution, Kafka stores change log information that's used to replicate the data stores. |Mainframe|
|[Partitioning in Event Hubs and Kafka](../reference-architectures/event-hubs/partitioning-in-event-hubs-and-kafka.yml)|Learn about partitioning in Kafka and Event Hubs for Kafka. Learn how many partitions to use in ingestion pipelines and how to assign events to partitions.|Analytics|
|[Patterns and implementations for a banking cloud transformation](../example-scenario/banking/patterns-and-implementations.yml)|Learn about the patterns and implementations used to transform a banking system for the cloud. A Kafka scaler is used to detect whether the solution needs to activate or deactivate application deployment.|Serverless|
|[Publisher-Subscriber pattern](../patterns/publisher-subscriber.yml)|Learn about the Publisher-Subscriber pattern, which enables an application to announce events to many interested consumers asynchronously. Kafka is recommended for messaging.|Integration|
|[Rate Limiting pattern](../patterns/rate-limiting-pattern.yml)|Use a rate limiting pattern to avoid or minimize throttling errors. This pattern can implement Kafka for messaging.|Integration|
|[Refactor mainframe applications with Advanced](../example-scenario/mainframe/refactor-mainframe-applications-advanced.yml)|Learn how to use the automated COBOL refactoring solution from Advanced to modernize your mainframe COBOL applications, run them on Azure, and reduce costs. Kafka can be used as a data source.|Mainframe|
|[Stream processing with fully managed open-source data engines](../example-scenario/data/open-source-data-engine-stream-processing.yml)|Stream events by using fully managed Azure data services. Use open-source technologies like Kafka, Kubernetes, Cassandra, PostgreSQL, and Redis components.|Analytics|

## Apache MapReduce

|Architecture|Summary|Technology focus|
|--|--|--|
|[Asynchronous messaging options](../guide/technology-choices/messaging.yml)|Learn about asynchronous messaging options in Azure. You can use MapReduce to generate reports on events captured by Event Hubs.|Integration|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. You can use MapReduce for batch processing and to provide functionality for parallel operations in these scenarios.|Databases|
|[Choose a batch processing technology](../data-guide/technology-choices/batch-processing.md)|Learn about technologies for big data batch processing in Azure, including HDInsight with MapReduce.|Analytics|
|[ETL using HDInsight](../solution-ideas/articles/extract-transform-and-load-using-hdinsight.yml)|ETL big data clusters on demand by using HDInsight, Hadoop MapReduce, and Apache Spark.|Analytics|
|[Geode pattern](../patterns/geodes.yml)|Deploy back-end services into a set of geographical nodes, each of which can service any client request in any region. This pattern occurs in big data architectures that use MapReduce to consolidate results across machines.|Databases|
|[Minimize coordination](../guide/design-principles/minimize-coordination.yml)|Follow these recommendations to improve scalability by minimizing coordination between application services. Use MapReduce to split work into independent tasks.|Databases|

## Apache NiFi

|Architecture|Summary|Technology focus|
|--|--|--|
|[Apache NiFi on Azure](../example-scenario/data/azure-nifi.yml)|Automate data flows with Apache NiFi on Azure. Use a scalable, highly available solution to move data into the cloud or storage and between cloud systems.|Analytics|
|[Helm-based deployments for Apache NiFi](../guide/data/helm-deployments-apache-nifi.yml)|Use Helm charts when you deploy NiFi on AKS. Helm streamlines the process of installing and managing Kubernetes applications.|Analytics|
|[Azure Data Explorer monitoring](../solution-ideas/articles/monitor-azure-data-explorer.yml)|Use Azure Data Explorer and NiFi in a hybrid monitoring solution that ingests streamed and batched logs from diverse sources.|Analytics|

## Apache Oozie

|Architecture|Summary|Technology focus|
|--|--|--|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. You can use Oozie for orchestration in these scenarios.|Databases|
|[Choose a data pipeline orchestration technology](../data-guide/technology-choices/pipeline-orchestration-data-movement.md)|Learn about the key orchestration capabilities of Oozie.|Databases|

## Apache Solr

|Architecture|Summary|Technology focus|
|--|--|--|
|[Choose a search data store](../data-guide/technology-choices/search-options.md)|Learn about the capabilities of search data stores in Azure and the key criteria for choosing one that best matches your needs. Learn about the key capabilities of HDInsight with Solr.|Databases|

## Apache Spark

|Architecture|Summary|Technology focus|
|--|--|--|
|[Advanced analytics](../solution-ideas/articles/advanced-analytics-on-big-data.yml)|Learn how you can combine any data at any scale with custom machine learning and get near real-time data analytics on streaming services. Use Spark pools to clean and transform structureless datasets and combine them with structured data.|Analytics|
|[AI at the edge with Azure Stack Hub](../solution-ideas/articles/ai-at-the-edge.yml)|Bring your trained AI model to the edge with Azure Stack Hub. Integrate it with your applications for low-latency intelligence. Use Spark to store data.|AI|
|[AI at the edge with Azure Stack Hub - disconnected](../solution-ideas/articles/ai-at-the-edge-disconnected.yml)|Take advantage of edge AI when disconnected from the internet and move your AI models to the edge with a solution that includes Azure Stack Hub. Use Spark to store data.|AI|
|[Analytics end-to-end with Azure Synapse](../example-scenario/dataplate2e/data-platform-end-to-end.yml)|Learn how to use Azure Data Services to build a modern analytics platform capable of handling the most common data challenges. The Spark Pools analytics engine is available from Azure Synapse workspaces. |Analytics|
|[Batch scoring of Spark on Azure Databricks](../ai-ml/architecture/batch-scoring-databricks.yml)|Build a scalable solution for batch scoring an Apache Spark classification model.|AI|
|[Big data analytics on confidential computing](../example-scenario/confidential/data-analytics-containers-spark-kubernetes-azure-sql.yml)|Use confidential computing on Kubernetes to run big data analytics with Spark inside confidential containers that are protected by Intel Software Guard Extensions.|Databases|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. You can use Spark for batch or stream processing and as an analytical data store.|Databases|
|[Campaign optimization with HDInsight Spark](/azure/architecture/solution-ideas/articles/optimize-marketing-with-machine-learning)|Build and deploy a machine learning model to maximize the purchase rate of leads that are targeted by a marketing campaign.|Databases|
|[Choose a batch processing technology](../data-guide/technology-choices/batch-processing.md)|Compare technology choices for big data batch processing in Azure, including options for implementing Spark.|Analytics|
|[Choose a stream processing technology](../data-guide/technology-choices/stream-processing.md)|Compare options for real-time message stream processing in Azure, including options for implementing Spark.|Analytics|
|[Choose an analytical data store](../data-guide/technology-choices/analytical-data-stores.md)|Evaluate analytical data store options for big data in Azure. Learn about the capabilities of Azure Synapse Spark pools.|Analytics|
|[Customer 360 with Azure Synapse and Dynamics 365 Customer Insights](../example-scenario/analytics/synapse-customer-insights.yml)|Build an end-to-end Customer 360 solution by using Azure Synapse Analytics and Dynamics 360 Customer Insights. This solution uses Azure Synapse Spark clusters, which can be scaled up and down automatically.|Analytics|
|[Data science and machine learning with Azure Databricks](../solution-ideas/articles/azure-databricks-data-science-machine-learning.yml)|Improve operations by using Azure Databricks, Delta Lake, and MLflow for data science and machine learning. Develop, train, and deploy machine learning models. Azure Databricks provides managed Spark clusters.|AI|
|[Extract, transform, and load](../data-guide/relational-data/etl.yml)|Learn about extract-transform-load (ETL) and extract-load-transform (ELT) data transformation pipelines and how to use control flows and data flows. In ELT, you can use Spark to query source data. You can also use it together with Hadoop as a data store.|Databases|
|[ETL using HDInsight](../solution-ideas/articles/extract-transform-and-load-using-hdinsight.yml)|ETL big data clusters on demand by using HDInsight, Hadoop MapReduce, and Apache Spark.|Analytics|
|[IoT using Azure Cosmos DB](../solution-ideas/articles/iot-using-cosmos-db.yml)|Learn how to use Azure Cosmos DB to accommodate diverse and unpredictable IoT workloads without sacrificing ingestion or query performance. Azure Databricks, running Spark Streaming, processes event data from devices. |IoT|
|[Loan charge-off predictions with HDInsight Spark](../solution-ideas/articles/loan-chargeoff-prediction-with-azure-hdinsight-spark-clusters.yml)|Use HDInsight and machine learning to predict the likelihood of loans getting charged off.|Databases|
|[Many models machine learning with Spark](../ai-ml/idea/many-models-machine-learning-azure-spark.yml)|Learn about many models machine learning in Azure.|AI|
|[Microsoft machine learning products](../ai-ml/guide/data-science-and-machine-learning.md)|Compare options for building, deploying, and managing your machine learning models, including the Azure Databricks Spark-based analytics platform and SynapseML. |AI|
|[Modern data warehouse for small and medium businesses](../example-scenario/data/small-medium-data-warehouse.yml)|Use Azure Synapse, Azure SQL Database, and Azure Data Lake Storage to modernize SMB legacy and on-premises data. Tools in the Azure Synapse workspace can use Spark compute capabilities to process data.|Analytics|
|[Natural language processing technology](../data-guide/technology-choices/natural-language-processing.yml)|Choose a natural language processing service for sentiment analysis, topic and language detection, key phrase extraction, and document categorization. Learn about the key capabilities of Azure HDInsight with Spark.|AI|
|[Observability patterns and metrics](../databricks-monitoring/databricks-observability.yml)|Learn how to use observability patterns and metrics to improve the processing performance of a big data system by using Azure Databricks. The Azure Databricks monitoring library streams Spark events and Spark Structured Streaming metrics from jobs.|Databases|
|[Real-time analytics on big data architecture](../solution-ideas/articles/real-time-analytics.yml)|Get deep-learning analytics and insights from live streaming data. Run advanced analytics on IoT device data and website clickstream logs in near real time. Apache Spark pools clean, transform, and analyze the streaming data and combine it with structured data.|Analytics|
|[Stream processing with fully managed open-source data engines](../example-scenario/data/open-source-data-engine-stream-processing.yml)|Stream events by using fully managed Azure data services. Use open-source technologies like Spark, Kafka, Kubernetes, Cassandra, PostgreSQL, and Redis components.|Analytics|

## Apache Sqoop

|Architecture|Summary|Technology focus|
|--|--|--|
|[Big data architectures](../databases/guide/big-data-architectures.yml)|Learn about big data architectures that handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems. In these scenarios, you can use Sqoop to automate orchestration workflows.|Databases
|[Choose a data transfer technology](../data-guide/scenarios/data-transfer.md)|Learn about data transfer options like Azure Import/Export, Data Box, and Sqoop.|Databases|

## Apache ZooKeeper

|Architecture|Summary|Technology focus|
|--|--|--|
|[Apache NiFi on Azure](../example-scenario/data/azure-nifi.yml)|Automate data flows with NiFi on Azure. Use a scalable, highly available solution to move data into the cloud or storage and between cloud systems. In this solution, NiFi uses ZooKeeper to coordinate the flow of data.|Analytics|
|[Helm-based deployments for Apache NiFi](../guide/data/helm-deployments-apache-nifi.yml)|Use Helm charts when you deploy NiFi on AKS. Helm streamlines the process of installing and managing Kubernetes applications. In this architecture, ZooKeeper provides cluster coordination.|Analytics|
|[Rate Limiting pattern](../patterns/rate-limiting-pattern.yml)|Use a rate limiting pattern to avoid or minimize throttling errors. In this scenario, you can use ZooKeeper to create a system that grants temporary leases to capacity. |Integration|

## Related resources

- [Microsoft partner and non-open-source third-party scenarios on Azure](../guide/partner-scenarios.md)
- [Scenarios featuring Microsoft on-premises technologies](../guide/on-premises-microsoft-technologies.md)
- [Architecture for startups](../guide/startups/startup-architecture.md)
- [Azure and Power Platform scenarios](../solutions/power-platform-scenarios.md)
- [Azure and Microsoft 365 scenarios](../solutions/microsoft-365-scenarios.md)
- [Azure and Dynamics 365 scenarios](../solutions/dynamics-365-scenarios.md)
- [Azure for AWS professionals](../aws-professional/index.md)
- [Azure for Google Cloud professionals](../gcp-professional/index.md)
