### Container Image Standards - App Container (appc)

- The [App Container](https://github.com/appc/spec)(appc), one of the container runtimes implementing the appc specification is rkt. 
- The appc specification defines a container image format, how an application is packaged into a container image, a deployment mechanism and a runtime. 
- In addition to defining the Application Container Image (ACI) format for container images, the appc enables the user community to develop tools to build, validate, and convert container images to ACI image format, such as goaci, docker2aci, deb2aci, actool, acbuild, and oci2aci. 


- The appc [specification](https://github.com/appc/spec/blob/master/SPEC.md) intends to speed up the design and the deployment of a container while ensuring container image integrity through cryptographic signatures. Appc defines several independent, yet compossable, aspects of the application. 

### Aspect of the Application Container
#### App Container Image
- The [App Container Image](https://github.com/appc/spec/blob/master/spec/aci.md)(ACI) defines the packaging, compression and extraction methods of files that make up the container image together with the validation of container image's integrity. 
- Individual files are packaged together into ACI archives that ensure not only fast and easy build, but also safe storage when downloaded and extracted to be saved on a system. 

- The ACI describes the following aspects of a container image:
	- File system layoout of the container image with its directory tree. 
	- Format of the container image archive, together with compression and encryption options. 
	- Unique image ID that allows its integrity verification, where the image ID is obtained by hashing the uncompressed ACI archive file. 
	- Image manifest file which describes the image and includes attributes such as the container name, specification version, users, exec command, file system, environment, resources, mount points, ports, dependencies, and metadata. 

#### App Container Image Discovery
- The [App Container Image Discovery](https://github.com/appc/spec/blob/master/spec/discovery.md) defines how a container image name is linked to a downloadable container image. The container image name format is similar to a URL, however, there is no scheme to resolve it. 
- The ACI discover process defines the usage of the name in conjunction with attributes from the image manifest file when retrieving a particular container image. 

- In order to address different URL types such as image, signature and public key, the specification describes the following steps in the ACI discovery process:
	- Discovery templates to render images and signature URLs,
	- Discovery URL templates to include an image or signature URL together with a URL to the keys required for ACI signature validation. 
	- Validation rules for ACI signature (option),
	- Authentication when retrieving container imagesi

#### App Container Pod 
- The [App Container Pod](https://github.com/appc/spec/blob/master/spec/pods.md) defines a Pod as the deployment and execution unit for one or a group of container images.
- A Pod represents an execution context obtained by namespacing various aspects and resources of the Linux Operating System: 
	- PID namespace where app containers can talk to each other, 
	- Network namespace where an IP address is shared between app containers, 
	- IPC namespace for app containers communication, 
	- UTS namespace for app containers to share a hostname. 
- A Pod is defined by a manifest which is a template listing the app containers to be grouped at execution time with their dependencies and isolators for resource constraints management. 


#### App Container Executor 
- The [App Container Executor](https://github.com/appc/spec/blob/master/spec/ace.md) (ACE) defines how to run an app container image, more specifically environment configuration for the running app, and the app's interaction with the environment. 
- Part of the app's environment, ACE must set the following:
	- UUID of Pod for unique identification within a domain, 
	- File System setup as a chrooted environment for each app container part of the Pod,
	- Volume setup and mounted at specified mount points inside the apps, 
	- Network setup with a loopback interface and optional IP layer interfaces,
	- Logging of apps is expected to stdout and stderr, which then ACE is able to capture and persist. 
- The app's interaction with the environment is defined by specifying:
	- Execution environment such as PATH, app name from the image manifest, metadata service URL and container name,
	- Linux isolators specific for seccomp. SELinux, and AppArmor define app permissions and capabilities,
	- Resource Isolators to enforce resource constraints for CPU and RAM at Pod level, at individual app level, or a mix of both.

### Container Image Standards - Open Container Initiative (OCI)

- The [Open Container Initiative](https://www.opencontainers.org/) (OCI) 
- One of the container runtimes implementing the OCI specification is runC 

- The OCI incorporates two specifications: 
	The Runtime Specification (runtime-spec) and 
	The Image Specification (image-spec).
- The [Runtime Specification](https://github.com/opencontainers/runtime-spec) defines how to run a "filesystem bundle" that is unpacked on disk. 
- An OCI implementation would download and unpack an OCI image into an OCI Runtime filesystem bundle. Then, an OCI Runtime would run the OCI Runtime Bundle. 
- The [Image Specification](https://github.com/opencontainers/image-spec) helps with the development of compatible tools to ensure consistent container image conversion into containers. 

#### OCI Specification
##### Runtime Specification 
- The OCI [Runtime Specification](https://github.com/opencontainers/runtime-spec) specifies the configuration, execution development, and lifecycle of a container. 
- The container's configuration file specifies how a container is created. The execution environment is specified to offer environment consistency between container runtimes. 
- A standard container, as referred to by the OCI, encapsulates a software component with its dependencies in a portable format, run by any OCI complaint runtime regardless of the underlying host machine and the contents of the container. 
- Standard Containers are defined based on the following principles:
	- Set of standard operations to be performed by various standard tools:
		- Start, stop, and create for container tools,
		- copy and snapshot for filesystem tools,
		- download and upload for network tools,
	- Content-agnostic operations have the same effect regardless of content, 
	- Infrastructure-agnostic to run in any OCI supported infrastructure,
	- Designed for automation with same standard operations regardless of content and infrastructure,
	- Industiral-grade delivery in-house DevOps or external customer-based software delivery mechanism. 
- The OCI Runtime Specification defines how container files are organized and what data and metadata they should include.                                                             
