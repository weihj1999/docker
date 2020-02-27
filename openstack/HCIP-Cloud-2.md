# 1. Compute Virtualization Concepts and technologies

## 1.1 Compute Virtualization Concepts

### 1.1.1 What's Virtualization?

- Virtualization is an abstracts layer that separates operating systems (OSs) from physical hardware.
- Virtual infrastructure is an enterprise-level which provides steady and powerful computation capability, maximizing resource usage and minimizing costs.
- VMs are important elements of virtual infrastructure. Virtualization allows multiple VMs with different OSs and applications installed to run on the same physical machines independently and concurrently.
- With virtualization, you can Dynamically adjust resources and processing capabilities as needed.

![虚拟化](images/hicp-cloudcompu-013.PNG)

- Virtualization is the basis of cloud computing. Virtualization enables one physical server to run multiple VMs. The VMs share the CPU, memory, and I/O hardware resources of the physical server. However, the VMs are logically isolated from each other.
- In compute science, virtualization refers to the creation of computing resources, that is, physical computing resources are virtualized into on or more operating environments. Virtualization software implements simulation, isolation, and sharing of resources.
- In essence, virtualization is a method in which a lower-layer software module provides an interface that is completely consitent with an expected operating environment to an upper-layer software module, and abstracts a virtual software or hardware interface, so that the upper-layer software module can directly run in the virtual environment. through virtualization, a resource can be abstracted into on or more parts by means of space division, time division, and simulation.

### 1.1.2 Essense of Virtualization

![虚拟化](images/hicp-cloudcompu-014.PNG)

- Partitioning: VMM can allocate server resources to multiple VMs. Each VM can run and independent OS (which can be the same as or different from the OSs running on other VMs on the same server) so that multiple applications can coexist on one server. Each OS gains access only to its own virtual hardware (Including the virtual NIC, virtual CPUs, and virtual memory) provided by VMM.
- Isolation: VMs that run on the same server are isolated from each other.
  - even if one VM breaks down or fails due to an OS failure, application breakdown, or driver failure, other VMs can still run properly.
  - From a practical perspective, it is as if each VM is located on an independent physical machine. If a VM is infected with a worm or virus, other VMs on the same physical machine are protected, due to VM isolation.
  - Resources can be managed to provide performance isolation. specifically,, you can specify the maximum and minimum resource usage for each VM to ensure that one VM does not use all the resources, leaving no available resources for other VMs in the same system.
  -multiple loads, applications, or OSs can run concurrently on one physical server, preventing problems that may occur on the x86 server, such as application program conflicts or DLL conflicts.
- Encapsulation: stores all VM data, Including the hardware configuration, BIOS configuration, memory status, disk status in a group of files that are independent of physical hardware. this enables user to clone, save and migrate a VM by copying, saving, and migrating a few files.
- Hareware independence: VMs runs on the virtualization layer. Therefore, only virtual hardware provided by the virtualization layer can be viewed. The virtual hardware is not aware of the physical server. In this way, the virtual machine can run on any x86 server(IBM, Dell, HP, etc) without any application. This frees user from the constraints that exist when OSs are tied to specific hardware, or applications are tied to specific OSs/hardware.

### 1.1.3 Compute Virtualization technologies

Categoried by Virtualization objective
- CPU Virtualization <br>
Processor Virtualization ensures that commands on VMs can be properly executed, and the execution efficiency is close to that of physical server
- Memory Virtualization <br>
Memory Virtualization allows each VM to have an independent memory address and ensures memory efficiency approaching that of physical server
- I/O Virtualization <br>
I/O Virtualization allows VMs to access required I/O resources, isolates I/O resources, and lowers virtualization costs.

Categoried by Virtualization process
- Full Virtualization<br>
VMM implements Virtualization of CPUs, memory, and device I/O without modification of guest OSs or hardware. Full Virtualization provides high compatibility, but imposes extra overhead on processors.
- ParaVirtualization<br>
The paraVirtualization, the vMM implements CPU and memory Virtualization. The guest OS implements device I/O Virtualization. The guest OS need to be modified, so that it can coordinate with the VMM. ParaVirtualization provides high performance and poor compatibility.
- Hardware-assisted Virtualization<br>
Hardware-assisted Virtualization is a platform Virtualization approach that enables efficient full Virtualization with the help of hardware, primarily the host processor. In hardware-assisted Virtualization, guest OSs are not mofified and good compatibility is provided. This technology is the dominant trend in Virtualization development and will put an end to the differences between software Virtualization technologies.

## 1.2 CPU Virtualization

### 1.2.1 CPU Virtualization Principles - Virtualization Challenges

- CPU Virtualization resolves the following issues:
  - simulation of CPU Instroduction (all sensitive instructions)
    - sensitive instructions: instructions that can read and write key system resources are called sensitive instructions
    - Privileged instructions: The majority of sensitive instructions are Privileged instructions, which can only be executed at the highest privilege level (kernel mode) of the processor.
  - Enabling multiple VMs to share CPUs
    - CPU virtualization uses a timer interruption mechanism similar to the time interruption mechanism used in native OSs. The timer interruption mechanism triggers the enabling of the VMM when an instruction occurs.  The VMM then schedules resources in accordance with the present scheduling policy.
- Key system resources: the interfaces presented by processors to software instruction sets and register. The interfaces presented by I/O devices to software are status and control registers, collectively called system resources. registers that affect the status and behavior of processors and deivces are called key system resources.

### 1.2.2 CPU Virtualization

FusionCompute compute virtualization uses the KVM technology. CPU virtualization of KVM is a CPU-assisted full virtualization, which requires the support of CPU virtualization Features.

![虚拟化](images/hicp-cloudcompu-015.PNG)

- FusionCompute uses hardware-assisted full virtualization
- The x86 OS is designed to run directly on raw hardware devices and therefore is considered to fully occupy computer hardware. The x86 Arichitecture provides four privilege levels for OSs and applications to access hardware. Ring indicates the CPU running level. Ring O is the highest level, Ring 1 is the second highest level.
  - A Linux or x86 OS (kernel) needs to directly access hardware and memory. <br>
  The OS code needs to run on the highest level (Ring 0) so that the OS can use Privileged instructions to control interruption, modify page tables, and access devices.
  - The code of applications runs at the lowest running level (Ring 3), and controlled operations are not allowed. If you want to perform contolled operations (for example, access disks or write files), you need execute system calls(functions). During systems calls, the CPU running level is switched from Ring 3 to Ring 0, and the system calls the corresponding kernel code. This way, the kernel completes device access can also be described as switching the user mode and kernel mode.
- However, this way of working gives rise to a problem. If the host OS is oeprating Ring 0, the guest OS can not operate Ring 0. However, the guest OS cannot detect that the host OS is oeprating Ring 0. An error occurs if the guest OS does not have the permission to execute certain instructions it has previously executed. In this situation, the VMM is needed to resolve this problem. The VMM allows VM guest CPUs to access hardware based on the following three technologies:
  - Full Virtualization
  - ParaVirtualization
  - Hardware-assisted Virtualization

### 1.2.3 CPU shared by VMs

**1. CPU can be shared by VMS**

CPU Virtualization uses a timer interruption mechanism that is similar to the timer interruption mechanism applied in native OSs. The timer interruption mechanism enables the VMM when an interruption occurs. The VMM then schedules resources in accordance with the preset scheduling policy.

![虚拟化](images/hicp-cloudcompu-016.PNG)

**2. Mapping between CPUs and vCPUs**

![虚拟化](images/hicp-cloudcompu-017.PNG)

- The above figure shows the mapping between the numbers of vCPUs and the number of physical CPUs.
- As an example, let's assume that this is an RH2288H V3 server using 2.6 GHz CPUs. A single server has two physical CPUs, each of which has eight cores. The Hyper-Threading technology provides two processing threads for each physical core. Therefore, each CPU has 16 threads, and the total number of vCPUs is 32 (2 x 8 x 2). This means the total CPU frequency is 83.2 GHz (32 x 2.6 GHz)
- The number of vCPUs on a VM cannot exceed the number of available vCPUs on a CNA node. However, because multiple VMs can use or share the same physical CPU， the total number of vCPUs running on a CNA node can exceed the actual number of vCPUs

## 1.3 Memory Virtualization

### 1.3.1 Memory Virtualization Challenges

- through memory management, a traditional native OS will ensure the following:
  - the memory satart from physical address 0.
  - Memory blocks have contiguous addresses.
- This approach to memory management gives rise to two issues:
  - Start from physical address 0: There is only one physical address 0, which cannot meet multiple concurrent customer requirements.
  - contiguous addresses: Although consecutive physical addresses can be allocated, this method of memory allocation leads to poor efficiency and flexibility.
- Memory virtualization resolves both issues.

### 1.3.2 Memory Virtualization

Memory virtualization refers to centrally managing physical memory of a PM and dividing the physical memory into multiple virtual memory spaces for multiple VMs. KVM uses memory virtualization to share the physical system memory and Dynamically allocates memory to VMs.

![虚拟化](images/hicp-cloudcompu-018.PNG)

In KVM, the physical memory of a VM is the memory space occupied by the QEMU-KVM process. KVM uses CPU-assisted memory virtualization. On the Intel platform, memory virtualization is implemented using the Extended Page Tables (EPT) technology.

![虚拟化](images/hicp-cloudcompu-019.PNG)
- H: Host
- G: Guest
- V: Virtual
- P：Physical
- A：addresses

### 1.3.3 Shadow Page Tables

The host machine MMU cannot directly load the page tables of guest machines for memory access. Therefore, multiple address translations are required when a guest machine accesses the physical memory of the host machine. specifically, the guest virtual address (GVA) is first translated in to the guest physical address (GPA) according to the guest page table. Then, the GPA is mapped and translated into the host virtual address (HVA).  The HVA is finally translated into the physical address (HPA) according to the host page table. With Shadow page tables, a GVA can be diredctly translated into an HPA.

On the other hand, Intel CPUs provide the Extended Page Tables (EPT) technology go support the following translation on the hardware:GVA->GPA->HPA, thereby simplifying the implementation of memory virtualization and improving memory virtualization performance.

AMD provides a technology called Nested Page Tables (NPT). This works in a similar manner to EPT.

### 1.3.4 Transparent Huge Page (THP)

![虚拟化](images/hicp-cloudcompu-020.PNG)

By default, the CPUs of x86 Arichitecture (including x86-32 and x86-64)
use 4KB memory pages and also support larger memory pages. For example, the x86-64 system supports 2 MB pages， Linux 2.6 and later versions supports huge pages. If huge pages are used in the system, the number of memory pages is reduced. This reduces the number of required page tables, memory pages occupied by the page tables, required address translations, and the number of times the translation Lookaside Buffer (TLB) becomes in effective, thereby improving  memory access performance. In addition, because information required for address translation is generally stored in CPU caches, the use of huge pages reduce such information, thereby reducing the nubmer of required CPU caches and load on them. This enables more CPU caches to be used for data cache of an application and improves the overall system performance.

## 1.4 I/O Virtualization

FusionCompute I/O virtualization implements two functions:
- Device discovery
  - FusionCompute controls which devices can be accessed by VMs.
- Access interception
  - VM access devices through I/O parts or MMIOs.
  - The device exchanges data with the memory through DMA.

I/O virtualization can be considered a hardware middleware layer between the system of a server Component and various available I/O processing units, allowing multiple guest OSs to reuse limited peripheral resources.

Device virtualization (I/O virtualization) simulates the registers and memory of devices, intercepts guest OS access to the I/O ports and registers, and uses software to simulate device behavior.

In QEMU/KVM, guest machine can use the following devices:
  - simulated devices: devices that are completely simulated by the QEMU software.
  - Virtio devices: paraVirtualized devices that implement VIRTIO APIs
  - PCI devics: directly assigned devices.

### 1.4.1 I/O Virtualization - Full Simulation

![io virtualization](images/hicp-cloudcompu-021.PNG)

1. Use software to simulate a specific device.
- The same software interface is used, for example: PIO, MMIO, DMA or interrupt.
- Virtual devices that are different from physical devices in the system can be simulated.
2. multiple context switches are required for each I/O operation.
- VM <-> Hypervisor
- QEMU <-> Hypervisor
3. Devices simulated by software do not affect the software stack of VMs.
- Native driver

- The advantages of Virtio paraVirtualized are as follows: It implements Virtio API, reduces the number of VM-Exits, and improve the execution efficiency of the guest machine I/O, which is much higher than that of the common simulation I/O.

- It also has the following disadvantages:
  - 1. its compatibility is low because the Virtio driver requires the support of the guest machine. (The earlier Linux OSs do not have the Virtio driver by default, and the Virtio driver must be additionally installed in the Windows OS.)
  - 2. When I/O operations are frequent, the CPU usage is high.

### 1.4.2 I/O Virtualization - Virtio

![io virtualization](images/hicp-cloudcompu-022.PNG)

Virtualizes special devices. Features include:
- Special device drivers, including the frontend driver on VMs and backend driver on hosts
- efficient communication between the frontend and backend drivers

Reduces the data transmission overhead between VMs and hosts, using three method:
- Shared memory (Virtio Ring)
- Batched I/O
- asynchronous event notification mechanism (waiting/notification) between Eventfd lightweight processes

The advantages of Virtio ParaVirtualization are as follows:
- It implements Virtio APIs, reduces the number of VM-Exits, and improves the execution efficiency of the guest machine I/O which is much higher than that of the common simulation I/O.
- It also has the following disadvantages:
  - 1. its compatibility is low because the Virtio driver requires the support of the guest machine. (The earlier Linux OSs do not have the Virtio driver by default, and the Virtio driver must be additionally installed in the Windows OS.)
  - 2. When I/O operations are frequent, the CPU usage is high.

### 1.4.3 PCI Device Assignment

KVM VMs allow the PCI and PCI-E devices in the host machine to the guest VM so that the guest VM can exclusively access the PCI or PCI-E devices. After a device has been assigned to a guest VM by using the VT-d technology supported by the hardware, the guest VM treats the device as if it is physically connected to the VMs PCI or PCI-E bus, and the I/O interaction between the guest VM and the device is no different from interaction between two physical devices. The Hypervisor rarely needs to participate in this process.

![io virtualization](images/hicp-cloudcompu-023.PNG)

- PCI deivces assigenment enables guest machines to fully occupy PCI devices. In this way, when I/O operations are performed, the number of VM-Exits is greatly reduced, so that the VM-Exits do not get trapped in the Hypervisor. This greatly improve the I/O performance and in fact achieves almost the same performanceas a non-virtualized system. Although the performance of Virtio is good, VT-d overcomes the problems of poor compatibility and high CPU usage.
- However, VT-d has its own disadvantages. Space on a server mainboard is limited, and the number of PCI and PCI-E devices that can be added is limited. If a host machine has a large number of guest machines, it is difficult to allocate VT-d devices to each guest machine independently. In addition, a large number of VT-d devices are independently assiged to guest machines, increasing the number of hardware devices and hardware investments costs.

## 1.5 Instroduction to FusionCompute Virtualization

### 1.5.1 FusionCompute Compute Virtualization Management

![compute virtualization](images/hicp-cloudcompu-024.PNG)

# 2. Compute Virtualization Features

## 2.1 Compatible with Various Special OSs

![compute virtualization](images/hicp-cloudcompu-025.PNG)

To be Compatible with a new OS, the PV driver must be provided by the vendor, Huawei has the PV driver R&D capability. In addition to the mainstream Windows and Linux OSs, FusionCompute is Compatible with NeoKylin OS. Certain OSs may need customized drivers.

- 350+ OSs
- PV driver: The virtualization driver of VMs, the PV driver is installed in the guest OS and used for OS optimization. This driver corresponds to VMWare tools.

## 2.2 Flexible Management Arichitecture

![compute virtualization](images/hicp-cloudcompu-026.PNG)

Technical Features and Benefits:
- Each logical cluster supports 128 PMs, and applies to high-performance and large-scale service groups deployed on PMs, reducing the redundancy ratio.
- Each logical cluster supports 8000 VMs and applies to service deployment on a large scale and with low performance requirements, for example, desktop clouds.
- HA design and VRM active/standby deployment (virtual deployment or physical deployment) ensure system availablity.

## 2.3 GPU Virtualization and GPU passthrough

![compute virtualization](images/hicp-cloudcompu-027.PNG)

application scenarios:
- applications such as mechanical design software (ProE, Catia, AutoCAD), media creation tool, 3D games, and GIS in a virtualized environment
- industrial design, multimedia editing , energy, finanacial services and trade, medical imaging system, education, etc.
- Improves the performance of demanding graphics and imaging applications running in a virtualized environment.

Key technologies and Features:
- Allows VMs to directly access some hardware resources of physical GPUs by binding to vGPUs.
- Provides GPU virtualization based on NVIDA GRID cards to improve graphics application experience.
- Supports vGPU resource management and scheduling to implement GPU load balancing scheduling.
- Supports multimedia programming interfaces OpenGL and DirectX.
- Supports AERO special effects, multi-monitor, and DXVA video hardware acceleration.

- GPU models supporting virtualization and passthrough: P4/P40/M60/V100
- GPU model supporting only passthrough: P100

## 2.4 Online CPU and Memory application

![compute virtualization](images/hicp-cloudcompu-028.PNG)

Technical Principles:
- vRAM and vCPUs can be added offline or online and deleted offline

Technical Highlights:
- Allows users to modify CPUs and memory sizes for a running VM. New settings take effect without VM restart.

Application scenarios:
- flexibility adjusts the number of CPUs and memory size of VMs according to service requirements.

Benefits:
- Flexible adjusts VM configuration based on the site requirements.
- Supports scale-up to ensure Qos of each VM.
- Integrates with the scale-out to ensure cluster QoS.

- Supports online CPU and memory modification for mainstream Linux VMs.
- Supports online memory modificationfor mainstream Windows VMs. modifications take effect after VMs are restarted.

## 2.5 Host Memory Overcommitment

- Host Memory and Guest Memory are not in a one-to-one relationship.
- Excess memory can be allocate to VMs.
- Memory Overcommitment supports such allocation:
  - For example, the total physical memory is 4GB, but the total memory allocated to the three upper-layer guest OSs reaches 6GB.
  ![compute virtualization](images/hicp-cloudcompu-029.PNG)

Three main scenarios about Memory:

- Memory sharing, copy-on-write<br>
Memory sharing: multiple VMs share the same physical memory block (marked in blue), and the VMs only read data from the memory block. Copy-on-write: When a VM needs to write data to its memory block, another memory block (marked in orange) is allocated to the VM to write the data and the mapping between the allocated block and the VM is created.<br>
![compute virtualization](images/hicp-cloudcompu-030.PNG)
- Memory swapping<br>
Memory swapping: Memory data that is not retrieved by the VM for a long time is swapped to a disk, and a mapping between the memory data and the disk is created. When the VM needs to access the memory data again, the memory data is returned from the disk back to the memory block.<br>
![compute virtualization](images/hicp-cloudcompu-031.PNG)
- Memory Bubble<br>
Memory Bubble: Hypervisor releases memory of idle VMs for use by VMs with higher memory requirements, improving memory usage. This is done using memory bubble technology.<br>
![compute virtualization](images/hicp-cloudcompu-032.PNG)

Technical Highlights:
- HUAWEI virtualization platform increases the memory Overcommitment ratio to 150% using the smart memory Overcommitment technology, which outperforms peer vendor C.

Benefits:
- Assuming equal memory resources, the VM density of a system that adopts memory Overcommitment is increased by 150% and the hardware purchase cost is reduced by 50%

Another important feature about memory: **NUMA Affinity scheduling**

![compute virtualization](images/hicp-cloudcompu-033.PNG)

NUMA：(Non Uniform Memory Access)即非一致内存访问架构。

NUMA具有多个节点(Node)，每个节点可以拥有多个CPU(每个CPU可以具有多个核或线程)，节点内使用共有的内存控制器，因此节点的所有内存对于本节点的所有CPU都是等同的，而对于其它节点中的所有CPU都是不同的。节点可分为本地节点(Local Node)、邻居节点(Neighbour Node)和远端节点(Remote Node)三种类型。

本地节点：对于某个节点中的所有CPU，此节点称为本地节点；

邻居节点：与本地节点相邻的节点称为邻居节点；

远端节点：非本地节点或邻居节点的节点，称为远端节点。

邻居节点和远端节点，称作非本地节点。

CPU访问不同类型节点内存的速度是不相同的：本地节点>邻居节点>远端节点。访问本地节点的速度最快，访问远端节点的速度最慢，即访问速度与节点的距离有关，距离越远访问速度越慢。考虑到上述问题，业务在部署虚拟机时很多情况下要求绑核。

## 2.6 VM HA

![compute virtualization](images/hicp-cloudcompu-034.PNG)  

- VM live migration is the basis of dynamic resources scheduling and distributed power management.
- Supports fault detection and recovery for hosts, virtualization platforms, and VMs.
- Supports centralized HA control and VRM-independent HA.
- Supports the configuration of the network plane for HA hearbeat messages to mitigate network pressure.
- Supports multiple fault diagnosis mechanisms to ensure accurate fault location.
- Supports VM HA using shared storage and local storage.

**VM Live Migration**

![compute virtualization](images/hicp-cloudcompu-035.PNG)  

**Dynamic Resource Scheduling (DRS)**

![compute virtualization](images/hicp-cloudcompu-036.PNG)

- DRS is also called compute resource scheduling automation.
- FusionCompute computing clusters are deployed on VIMS shared devices. DRS performs real-time monitoring of resource usage for each computing node in a cluster, and uses the VM live migration function to intelligently migrate VMs with high workloads to other nodes with sufficient resources, thereby balancing resource use and ensuring that resources are sufficient. DRS provides the foundation for automatic load balancing.

**Distributed Power Management (DPM)**

![compute virtualization](images/hicp-cloudcompu-037.PNG)


**IMC**

- Configure the IMC function for a cluster to enable VM migration across hosts that use CPUs of different performance baseline in the cluster.
- The IMC function allows VMs running on the hosts in the cluster to present the same CPU features. This enaures successful VM migration across these hosts even if the hosts use physical CPUs of different performance baselines.
- Ensure that the following conditions are met for the IMC function if the cluster contains hosts and VMs:
  - The CPU generations of the hosts in the cluster are the same as or earlier than the target IMC mode. If any vM in the cluster does not meet this requirement, the VM must be stopped or migrated to another cluster.
  - The CPU generations of the running or hibernated VMs in the cluster are the same as or earlier than the target IMC mode. If any VM in the cluster does not meet this requirement, the VM must be stopped or migrated to another cluster.


**Rule Gropp**

![compute virtualization](images/hicp-cloudcompu-038.PNG)
  
