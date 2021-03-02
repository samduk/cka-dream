## Virtualization Fundamentals 

### Control Groups (cgroups)
- feature of the Linux kernel 
	- allowing the limitation, accounting, and isolation of resources used by groups of processes and their subgroups.- Cgroups are a fundamental feature of various Operating System level virtualization mechanisms, such as OpenVZ and LXC. 

	- <img src="https://github.com/samduk/cka-dream/blob/master/images/cgroup.png" width="300" height="600"> 

- Although cgroups may control single processes, they are used in the Operating System level virtualization predominantly to manage multiple processes together (hence the 'groups' in cgroups). Cgroups allow the limitation of memory, disk I/O, and network usage for a group of processes. In addition, cgroups may set usage quotas, and prioritize a process group to receive more CPU time or memory than other groups. Also, a group's resource usage can be measured for accounting and billing purposes, and its state can be controlled by freezing and restarting the group. 

- Containers benefit from cgroups primarily because they allow system resources to be limited for processes grouped by a container. In addition, the processess of a container are treated and managed as a whole unit, and a container may be prioritized for preferrential resource allocation. 

### Namespaces 

- Namespaces are a feature of a linux kernel allowing groups of processes to have limited visibility of the host system resources. 

- Namespaces may limit the visibility of cgroups, hostname, process IDs, IPC mechanisms, network interfaces, and routes, users, and mounted file systems. To an isolated process running inside a namespace, a namespaced resource, such as the network, will appear as the process' own dedicated resource. Processes running inside a namespace are aware of any changes in the local namespaced system resources, however, such changes will not be visible to other processes or other namespaces. 

- In the context of containers, namespaces isolate processes from on container to prevent them from modifying the hostname, network interfaces, or mounts for processes running in other containers. The processes isolated inside a container can only see system resoures namespaced for that paricular container. Namespaces also help resolve PID namespaces, while at the global level of the host system they are each assigned different PIDs. As a result, multiple root processes are alloweded to run on the same host system, isolated in separate namespaces, a situation that otherwise would not be possible since root is unique on a host system.

### Unification File System (UnionFS)
- UnionFS is a feature found in Linux, FreeBSD, and NETBSD kernels, allowing the overlay of separate transparent file systems to produce an apparent single unified file system. 

- When the content of several file systems, called branches, are virtually stacked, their contents appear to be merged, however, physically, they remain separate. The real physical separation, together with read-only and read-write access modes, help to prevent data corruption with the implementation of a Copy-on_write (CoW) mechanism. This mechanism creates a phyiscal copy of an existing file when it is being accessed to be changed. A copy of the file is made onto a real writable file system, part of the unionfs, and that copy is in fact being modified. However, virtually through the unified file system, it appears as if the original file has been modified instead. 

	- <img src="https://github.com/samduk/cka-dream/blob/master/images/unionfs.png" width="300" height="600">

