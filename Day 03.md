## Virtualization Mechanisms 


- <img src="https://github.com/samduk/cka-dream/blob/master/images/virtualization%20and%20containers.png" width="300" height="600">

### Operating System-Level Virtualization 
- Operating System-Level Virtualization refers to a kernel's capability to allow the creation and existence of multiple isolated virtual environments on the same host. Such environments, known as containers, zones, partitions, virtual kernels, or jails, encapsulate running programs and conceal from them the true nature of their virtual environment, where they see all resources such as CPU, network, connected devices, and files, programs running inside a virtual environment are limited to its content and assigned devices. 

- A real Operating System may allow or deny access to resources, or it may hide resources from programs. An Operating System level virtualized environment may have allocated only a set of the available resources, thus limiting program's access to them. 

- On real OS, one or many isolated virtual environments may be created, with each running one or multiple programs. These programs may run concurrently, separately and may even interact with each other. 

- Operating System level virtualization is typically used to limit usage and securely islate resources shared between multiple programs or users, and to separate programs to run in their own assigned virtual environments for better security and resource management. 

- While Operating System level virtualization requires less overhead than full virtualization because everything is managed at the kernel level without the need to install a guest OS, it limits the OS of the virtual environemtn to the host Operating System. Also, the OS-level virtualization introduces a stacked storage management model, implemented by the file-level Copy-on-Write(CoW) mechanism. This CoW mechanism ensures minimal storage space usage when saving files that are being modified. Changes and new data are stored on disk, but data duplication is prevented by making heavy usage of links for referencing origianl data that remains unchanged.

### Virtualization Mechanisms 

#### Chroot
- Chroot is a mechanism implementing OS-level virtualization. 
- Chroot operates on a UNIX-like operating system by changing the apparent root directory of a process and its children. This apparent root directory is no longer the real root directory of the operating system, it is a virtual root directory - commonly referred to as a <strong>chrooted directory</strong>. Any user process and/or its children running inside this virtual chrooted directory tree runs under the false impression that it is in the real root system, without the possibility of escaping it to access the real root directory of the operating system. For this purpose, the notion of <b>chroot jail</b> was first introduced in 1991, when the feature was used to build a honeypot to monitor the activity of crackers. 

Chroot may be used to create a test environment to safely and securely test new software, to manage and isolate dependencies, and to recover and unbootable system.

While only a root user may perform a chroot, it is not a defence mechanism against root attacks. A second chroot may be performed by a program with elevated privileges to break out of the chroot jail. In order to avoid such escapes, FreeBSD jails should be used instead, because FreeBSD can prevent a second chroot type of attack. While chroot provides partial filesystem isolation and nested virtualization, it shares some system resources such as users, I/O, network bandwidth, hostname, IP address, disk space and CPU time.


### FreeBSD Jails 
- A FreeBSD jail is a mechanism implementing OS-level virtualization with very little overhead. 
- FreeBSD jail allows for the partitioning of a FreeBSD system into many independent systems, called <b>jails</b>. They share the same kernel, but virtualize the system's files and resources for improved security and administration through clean isolation between services. 
- Jails become virtual environments runnign on the host system with virtualized filesystem, processes, and users. A process running inside a jail runs under the false impression that it is the real root directory tree of the host system, useful to system administrators who can easily sandbox in jails the tasks needing superuser permissions without actually allowing them access to the real root user of the host system. 
- Jail allow for multiple isolated Virtual Machines to coexist on the same host, where each Virtual Machine may be customized and loaded with particular utilities. However, Virtual Machines cannot run different kernel versions from the host system - they share the host kernel instead. Jailed processes are restricted from communicating with processes from other jails or the host, loading additional kernel modules, modifying network interface configuration, and performing fielsystem operations to mount and unmount volumes. These processes are confined to the chrooted filesystem of the Virtual Machine with virtualized network interfaces, IP address, and users. 

### Solaris Zones
- A Solaris zone is a mechanism implementing OS-level virtualization.
- Solaris zones represent securely isolated Virtual Machines on a single host system. 
- Zones may host single or multiple applications, services, and their children. 
- Each zone on a host system virtualizes its hostname, network, IP address, and it has assigned storage. 
- Zones may have allocated dedicated physical resources such as CPU, memory, and physical network, but they require minimal storage for their configuration data. 
- Processes running inside a zone are isolated from other zones and the rest of the host system. Direct communication is possible among processes within the same zone, however, communication with processes from other zones is only allowed APIs. 
- Zones may be used to run multiple applications on a single host while reducing operating costs and management complexity. 


### OpenVZ
- OpenVZ is a mechanism implementing OS-level virtualization
- OpenVZ allows a physical host to run multiple isolated virtual instances - called containers, virtual environments or virtual private servers. 
- OpenVZ containers share the same kernel, and can only run Linux. 
- OpenVZ containers, although virtualized systems, behave just like real physical hosts. Each container has its own virtual filesystem, users, processes, and network. 
- By default, containers are hardware - independent because they do not have access to the real hardware of the host.   
- However, access can be enabled by administrators to some physical devices such as disks and network cards.

### Linux Containers (LXC)
- LXC, or Linux Containers, is a mechanism implementing OS-level virtualization.
- LXC allows multiple isolated systems to run on a single Linux host, using chroot and cgroups, together with namespace isolation features of the Linux kernel to limit resources, set priorities, and isolated processes, the filesystem, network and users from the host operating system. 

### Systemd-nspawn
- Systemd-nspawn is a mechanism implementing OS-level virtualization.
- Systemd-nspawn may be used to run a simple script or boot an entire Linux-like operating system in a container. 
- Systemd-nspawn full isolated containers from each other and from the host system, therefore processes running in a container are not able to communicate with processes from other containers. It fully virtualizes the process tree, filesystem, users, host and domain name. 
- Containers created by systemd-nspawn may run as system services, and may be managed with systemd - the default service manager that bootstraps the user space and manages user processes on many Linux distributions. 

 













 
