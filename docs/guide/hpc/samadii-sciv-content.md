This article briefly describes the steps for running [Samadii SCIV](https://www.metariver.kr/smdsciv.html) on a virtual machine (VM) that's deployed on Azure. It also presents the performance results of running Samadii SCIV on Azure.

Samadii SCIV (Statistical Contact in Vacuum) analyzes fluid behavior, deposition processes, and chemical reactions on rarefied gas regions by using the direct simulation Monte Carlo (DSMC) method. To calculate the physical phenomena represented by the Boltzmann equation, the DSMC method uses representative particles, which replace the real molecules. SCIV also provides functions for traditional flow simulation, display deposition processes, and semiconductor device analysis in rarefied gas regions. Samadii SCIV is based on a GPU architecture and uses CUDA technology.

SCIV is used by manufacturers of display devices and semiconductors, in aerospace and manufacturing, and in other industries.

## Why deploy Samadii SCIV on Azure?

- Modern and diverse compute options to meet your workload's needs
- The flexibility of virtualization without the need to buy and maintain physical hardware
- Rapid provisioning
- Strong performance scale-up, with configurations that provide either optimized scaling or optimized cost efficiency

## Architecture

:::image type="content" source="media/samadii-sciv/samadii-sciv-architecture.svg" alt-text="Diagram that shows an architecture for deploying Samadii SCIV." lightbox="media/samadii-sciv/samadii-sciv-architecture.svg" border="false":::

*Download a [Visio file](https://arch-center.azureedge.net/samadii-sciv.vsdx) of this
architecture.*

### Components

- [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines) is
    used to create a Windows VM. For information about deploying the VM and installing the drivers, see [Windows VMs on Azure](../../reference-architectures/n-tier/windows-vm.yml).
- [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network) is
    used to create a private network infrastructure in the cloud.
  - [Network security groups](/azure/virtual-network/network-security-groups-overview) restrict access to the VM.  
  - A public IP address connects the internet to the VM.
- A physical solid-state drive (SSD) provides storage.

## Compute sizing and drivers

The performance tests of Samadii SCIV on Azure used [NVv3](/azure/virtual-machines/nvv3-series), [NCasT4_v3](/azure/virtual-machines/nct4-v3-series), [NCv3](/azure/virtual-machines/ncv3-series), [ND_A100_v4](/azure/virtual-machines/nda100-v4-series), and [NC_A100_v4](/azure/virtual-machines/nc-a100-v4-series) series VMs running Windows 10. The following table provides details about the VMs.

|VM size| GPU name| vCPUs| Memory, in GiB| Maximum data disks| GPUs |GPU memory, in GiB| Maximum uncached disk throughput, in IOPS / MBps| Temporary storage (SSD), in GiB| Maximum NICs|
|-|-|-|-|-|-|-|-|-|-|
|Standard_NV12s_v3| Tesla M60| 12| 112| 12| 1| 8| 20,000 / 200| 320| 4|
|Standard_NC4as_T4_v3| Tesla T4 |4| 28| 8| 1 |16 |-| 180| 2|
|Standard_NC6s_v3 |V100| 6 |112| 12 |1 |16| 20,000 / 200| 736| 4|
|Standard_ND96asr_v4| A100| 96 |900 |32| 8 |40 |80,000 / 800 |6,000 |8|
|Standard_NC24ads_A100_v4|A100| 24 |220 |32 |1 |80 |30,000 / 1,000 |1,123 |2|

### Required drivers

To take advantage of the GPU capabilities of [NVv3](/azure/virtual-machines/nvv3-series), [NCasT4_v3](/azure/virtual-machines/nct4-v3-series), [NCv3](/azure/virtual-machines/ncv3-series), [ND_A100_v4](/azure/virtual-machines/nda100-v4-series), and [NC_A100_v4](/azure/virtual-machines/nc-a100-v4-series) series VMs, you need to install NVIDIA GPU drivers.

To use AMD processors on [NCasT4_v3](/azure/virtual-machines/nct4-v3-series), [NCv3](/azure/virtual-machines/ncv3-series), [ND_A100_v4](/azure/virtual-machines/nda100-v4-series), and [NC_A100_v4](/azure/virtual-machines/nc-a100-v4-series) series VMs, you need to install AMD drivers.

## Samadii SCIV installation

Before you install SCIV, you need to deploy and connect a VM and install the required NVIDIA and AMD drivers.

For information about deploying the VM and installing the drivers, see [Run a Windows VM on Azure](../../reference-architectures/n-tier/windows-vm.yml).

> [!IMPORTANT]
> NVIDIA Fabric Manager installation is required for VMs that use NVLink. ND_A100_v4 and NC_A100_v4 series VMs use this technology.

Following are some prerequisites for running Samadii applications.

- Windows 10 (x64) OS
- One or more NVIDIA CUDA-enabled GPUs: Tesla, Quadro, or GeForce series
- Visual C++ 2010 SP1 Redistributable Package 
- Microsoft MPI v7.1
- .NET Framework 4.5

The product installation process involves installing a license server, installing Samadii SCIV, and configuring the license server. For more information about installing SCIV, contact [Metariver Technology](https://www.metariver.kr/index.html).

## Samadii SCIV performance results

The following table shows the operating system versions and processors that were used for the tests.

|VM series| ND_A100_v4 |NCv3|  NCasT4_v3| NVv3|  NC_A100_v4|
|-|-|-|-|-|-|
|Operating system version|Windows 10 Professional, version 20H2|Windows 10 Professional, version 20H2|Windows 10 Professional, version 20H2|Windows 10 Professional, version 20H2|Windows 10 Professional, version 21H2|
|OS architecture|x86-64 |x86-64 |x86-64 |x86-64 |x86-64 |
|Processor |AMD EPYC 7V12, 64-core processor, 2.44 GHz (2 processors) |Intel Xeon CPU E5-2690 v4| AMD EPYC 7V12, 64-core processor, 2.44 GHz |Intel Xeon CPU E5-2690 v4| AMD EPYC 7V13, 64-core processor, 2.44 GHz|

The PNS model was used for testing:

:::image type="content" source="media/samadii-sciv/pns-model.png" alt-text="Screenshot that shows the PNS model that was used for testing.":::

- Size: 17,652
- Cell type: Shell 
- Solver: Samadii SCIV V21 R1
- Number of GPUs used for all simulations: One

The following table shows the elapsed runtimes and relative speed increases on each VM series. The NVv3 series VM is used as a baseline for the relative speed increases.

|VM series|Number of GPUs| Elapsed time, in seconds |Relative speed increase|
|-|-|-|-|
|NVv3|1|93,483.74 |N/A|
|NCasT4_v3|1 |38,311.8|2.44|
|NCv3|1 |27,096.83|3.45|
|ND_A100_v4|1 |16,322.98|5.73|
|NC_A100_v4|1 |40,895.55| 2.29|

This graph shows the relative speed increases:

:::image type="content" source="media/samadii-sciv/sciv-graph.png" alt-text="Graph that shows the relative speed increases." border="false":::

## Azure cost

The following table presents simulation runtimes in hours. To compute the total cost, multiply these times by the Azure VM hourly costs for NVv3, NCasT4_v3, NCv3, ND_A100_v4, and NC_A100_v4 series VMs. For the current hourly costs, see [Windows Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#pricing).

Only simulation runtime is considered in these cost calculations. Application installation time and license costs aren't included.

You can use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator) to estimate the costs for your configuration.

|VM size| Number of GPUs| Wall-clock time, in hours|
|-|-|-|
|Standard_NV12s_v3| 1 |25.97|
|Standard_NC4as_T4_v3| 1| 10.64|
|Standard_NC6s_v3 |1 |7.53|
|Standard_ND96asr_v4| 1 | 4.53|
|Standard_NC24ads_A100_v4| 1 |11.36|

## Summary

- Samadii SCIV was tested on ND_A100_v4, NCv3, NCasT4_v3, NVv3, and NC_A100_v4 series VMs.
- SCIV performance scales well on NCasT4_v3, NCv3, ND_A100_v4, and NC_A100_v4 series VMs.
- The NCasT4_v3 series VMs are more cost efficient than ND_A100_v4, NCv3, and NVv3 series VMs.
- Of the five VM series, ND_A100_v4 provides the best relative speed increase.

## Contributors

*This article is maintained by Microsoft. It was originally written by
the following contributors.*

Principal authors:

-   [Hari Bagudu](https://www.linkedin.com/in/hari-bagudu-88732a19) |
    Senior Manager
-   [Gauhar Junnarkar](https://www.linkedin.com/in/gauharjunnarkar) |
    Principal Program Manager
-   [Vinod Pamulapati](https://www.linkedin.com/in/vinod-reddy-20481a104) |
    HPC Performance Engineer

Other contributors:

-   [Mick Alberts](https://www.linkedin.com/in/mick-alberts-a24a1414) |
    Technical Writer
-   [Guy Bursell](https://www.linkedin.com/in/guybursell) | Director
    of Business Strategy
-   [Sachin Rastogi](https://www.linkedin.com/in/sachin-rastogi-907a3b5) |
    Manager

*To see non-public LinkedIn profiles, sign in to LinkedIn.*

## Next steps

- [GPU-optimized virtual machine sizes](/azure/virtual-machines/sizes-gpu)
- [Virtual machines on Azure](/azure/virtual-machines/overview)
- [Virtual networks and virtual machines on Azure](/azure/virtual-network/network-overview)
- [Learning path: Run HPC applications on Azure](/learn/paths/run-high-performance-computing-applications-azure)

## Related resources

- [Run a Windows VM on Azure](../../reference-architectures/n-tier/windows-vm.yml)
- [HPC system and big-compute solutions](../../solution-ideas/articles/big-compute-with-azure-batch.yml)