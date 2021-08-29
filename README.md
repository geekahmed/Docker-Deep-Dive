

This is my summary of the Docker Deep Dive, by Nigel Poulton. This book is considered a great introduction to Docker with practical exercises as found in the real-world projects.

Contributions: Issues, comments and pull requests are super welcome 😃

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->
# Table of Contents
- [Table of Contents](#table-of-contents)
- [Chapter 1. Containers from 30,000 feet]()
	- [1.-The bad old days]()
	- [2.-Hello VMware!]()
	- [3.-VMwarts]()
	- [4.-Hello Containers!]()
	- [5.-Linux containers]()
	- [6.-Hello Docker!]()
	- [7.-Windows containers]()
	- [8.-Windows containers vs Linux containers]()
	- [9.-What about Mac containers?]()
	- [10.-What about Kubernetes?]()
- [Chapter 2.  Docker]()
	- [1.-Docker - The TLDR]()
	- [2.-Docker, Inc.]()
	- [3.-The Docker runtime and orchestration engine]()
	- [4.-The Docker open-source project (Moby)]()
	- [5.-The container ecosystem]()
	- [6.-The Open Container Initiative (OCI)]()
- [Chapter 3. Installing Docker]()
	- [1.-Docker for Windows (DfW)]()
	- [2.-Docker for Mac (DfM)]()
	- [3.-Installing Docker on Linux]()
	- [4.-Installing Docker on Windows Server 2016]()
	- [5.-Upgrading the Docker Engine]()
	- [6.-Docker and storage drivers]()
- [Chapter 4. The big picture]()
	- [1.-The Ops Perspective]()
	- [2.-The Dev Perspective]()
- [Chapter 5. The Docker Engine](#chapter-5-the-docker-engine)
	- [1.-Docker Engine - The TLDR]()
	- [2.-Docker Engine - The Deep Dive]()
- [Chapter 6. Images](#chapter-6-images)
	- [1.-Docker Images - The TLDR]()
	- [2.-Docker Images - The Deep Dive]()
	- [3.-Images - The Commands]()
- [Chapter 7. Containers](#chapter-7-containers)
	- [1.-Docker Containers - The TLDR]()
	- [2.-Docker Containers - The Deep Dive]()
	- [3.-Containers - The Commands]()
- [Chapter 8. Containerizing an app](#chapter-8-containerizing-an-app)
	- [1.-Containerizing an app - The TLDR]()
	- [2.-Containerizing an app - The deep dive]()
	- [3.-Containerizing an app - The commands]()
<!-- /TOC -->

# Chapter 5. The Docker Engine
## Section 1: Docker Engine - The TLDR
 - The Docker engine is the core software that runs and manages
   containers.
 - The Docker engine is modular in design with many swappable
   components and based on the open-standards from the OCI.
 - The major components that make up the Docker engine are:
	 - Docker Client
	 - Docker Daemon
	 - Containerd
	 - Runc
## Section 2: Docker Engine - The Deep Dive
 - In Docker first days, the two major components are:
	 - The Docker Daemon: Monolithic binary contains all the code of Docker Client, API, Container Runtime, image manager, etc.
	 - LXC: Provided the daemon with the fundamental access of containers building-blocks like namespaces and cgroups.
 - LXC was a linux-specific and make problems with projects that tends
   to be cross-platform.
 - Docker. Inc. has developed its own tool called "libcontainer" instead
   of LXC.
 - "libcontainer" was a platform-agnostic tool providing Docker the
   basic container building-blocks inside the kernel
 - Docker. Inc. started to refactor the Docker engine which all of the
   container execution and container runtime code entirely removed from the daemon and refactored into small, specialized tools.
 - The Docker daemon implements the Docker API which is currently a
   rich, versioned, HTTP API that has developed alongside the rest of
   the Docker project.
 - Container execution is handled by containerd. containerd was written by Docker, Inc. and contributed to the CNCF. You can think of it as a container supervisor that handles container lifecycle operations. It is small and lightweight and can be used by other projects and third-party tools. For example, it’s poised to become the default, and most common, container runtime in Kubernetes.
 - containerd needs to talk to an OCI-compliant container runtime to actually create containers. By default, Docker uses runc as its default container runtime. runc is the de facto implementation of the OCI container-runtime-spec and expects to start containers from OCI- compliant bundles. containerd talks to runc and ensures Docker images are presented to runc as OCI-compliant bundles.
 - runc can be used as a standalone CLI tool to create containers. It’s based on code from libcontainer, and can also be used by other projects and third-party tools.
 - There is still a lot of functionality implemented in the Docker daemon. More of this may be broken out over time. Functionality currently still inside of the Docker daemon include, but is not limited to: the API, image management, authentication, security features, core networking, and volumes.
 - The work of modularizing the Docker engine is ongoing.

# Chapter 6. Images
## Section 1: Docker Images - The TLDR
 - A Docker is like a VM template or a class in OOP.
 - The images are pulled from a registery and the default is Docker Hub
 - Images are made up of multiple layers that get stacked on top of each
   other and represented as a single object.
## Section 2: Docker Images - The Deep Dive
 - Images are build-time constructs while containers are run-time constructs.
 - An image will not be deleted unless the last container uses it is stopped and destroyed.
 - Image registries are the storage area for the images.
 - Image registries contains multiple image repositories.
 - Image repositories can contain multiple images.
 - The latest tag doesn't gruarantee it is the most recent image in the repository.
 - Each image may be tagged with more than one tag.
 - Filtering images using `docker image ls --filter` which supports: dangling, before, since, and label.
 - A dangling image is an image that is no longer tagged and appears in listings as `< none >:< none >`
 - A Docker Image is just a bunch of loosely-connected read-only layers.
 - Docker employs a storage driver that is responsible for stacking layers and presenting them as a single unified filesystem.
 - Storage drivers on Linux includes AUFS, overlay2, devicemapper, btrfs, and zfs.
 - Multiple images can, and do, share layers. This leads to efficiencies in space and performance.
 - Each image has a content hash and a distrbution hash.
 - The content hash is the SHA256 of the image content.
 - The distrbution hash is the has for the compressed image.
 - Multi-architecture images has a manifest lists and manifests.
## Section 3: Images - The Commands

 - `docker image pull` is the command to download images.
 - `docker image ls` lists all of the images stored in your Docker
   host’s local cache. To see the SHA256 digests of images add the
   --digests flag.
 - `docker image inspect` gives you all of the glorious details of an
   image — layer data and metadata.
 - `docker image rm` is the command to delete images. You cannot delete an image that is associated with a container in the running (Up) or stopped (Exited) states.

# Chapter 7. Containers
## Section 1: Docker Containers - The TLDR
 - A container is the runtime instance of an image.
	 - Like starting a virtual machine from a virtual machine template or intializing an object from a class.
 - Containers run until the app they are executing exits.
## Section 2: Docker Containers - The Deep Dive
 - Containers an VMs both need a host to run.
	- Personal PC.
	- Bare-metal server.
	- The public cloud.
 - Hypervisors perform hardware virtualization while containers perform OS virtualization.
 - VM/OS tax is the resources used by the OS /VMs from the physical/virtual resources (CPU, RAM, Disk, etc.).
 - The containers share the OS kernel so it will pay one OS tax bill. On the other hand, every VM should install an OS so it will pay much more bill.
 - Containers start time are much faster compared with VMs start time.
 - Docker daemon implements the Docker Remote API on local IPC/Unix socket at `/var/run/docker.sock`.
 - It is possible to configure the Docker client and daemon to communicate over the network. The default non-TLS port is 2375, and the default TLS port is 2376.
 - Images are highly optimized fot containers. This means they don't have all of the normal commands and package installed.
 - Killing the main process in the container will also kill the container.
 - Containers could be stopped, started, restarted, and paused as many times as wanted.
 - There is no loss in container's data excepts if it is deleted.
	- If so, there is a concept called volume which persist data even if the container deleted.
 - It is a best practice to firstly stop the container then removing it as this way will give a chance to the process running inside the container to organize itself before the end.
	- Docker sends "SIGTERM" to the process inside it to exit and if not so, it sends "SIGKILL". Using the two-way approch.
 - Restart policy is a form of self-healing that enables Docker to automatically restart them after certain events or failures have occured.
 - Restart policies are applied per-container.
	- It can be configured imperatively on the cli.
	- It can be configured declaratively in Compose files or Docker stacks.
 - Always policy will always restart the stopped container unless it has been explicitly stopped using for example `docker container stop`. Also, it will restart the container if the daemon restarts.
 - Unless-stopped policy is like the always policy except it will not restart the container if the daemon restarted if they were in the "Stopped" state.
 - The on-failure policy will restart the container if it exits with a non-zero exit code. It will also restarts the container when the Docker daemon restarts, even containers that were in the stopped state.
 - `docker container run -d`. The -d flag called daemon mode which make the container to run in the background and not attach our shell to it.
 - The port mappings are expressed as `host-port:container-port`.
 - It is common to build images with default commands as it makes starting containers easier.
	- It also forces a default behavior and is a form of self-documentation for the image.
## Section 3: Containers - The commands
 - `docker container run` is the command used to start new containers.
 - `Ctrl-PQ` will detach your shell from the terminal of a container and leave the container running (UP) in the background.
 - `docker container ls` lists all containers in the running (UP) state.
	 - The -a flag you will also see containers in the stopped (Exited) state.
 - `docker container exec` lets you run a new process inside of a running container.
 - `docker container stop` will stop a running container and put it in the Exited (0) state. 
 -  `docker container start` will restart a stopped (Exited) container. You can give docker container start the name or ID of a container.
 - `docker container rm` will delete a stopped container.
	 - The -f flag will force removing a non-stopped container.
 - `docker container inspect` will show you detailed configuration and runtime information about a container. It accepts container names and container IDs as its main argument.

# Chapter 8. Containerizing an app
## Section 1: Containerizing an app - The TLDR
 - The process of taking an application and configuring it to run as a container is called containerizing.
 - The process of containerizing the app as follow:
	 - Start with the application code.
	 - Create Dockerfile that describes the app, its dependencies, and how to run it.
	 - Feed the Dockerfile into the `docker image build` command.
## Section 2: Containerizing an app - The deep dive
- The directory containing the application is referred to as the build context.
	- A common practice to keep Dockerfile in the root directory of the build context.
	- It is important that Dockerfile starts with capital "D" and is all one word.
- The purpose of Dockerfile:
	- Describe the application.
	- Tell Docker how to containerizw the application.
- Some instructions create new layers like:
	- FROM
	- RUN
	- COPY
- Others add just metadate like:
	- EXPOSE
	- WORKDIR
	- ENV
	- ENTRYPOINT
- If an instruction is adding content such as files and programs to the image, it will create a new layer.
	- If it is adding instructions on how to build the image and run the applications, it will create metadata.
- Leverage the build cache
	- Invalidating cache invalidates it for the remainder of the build.
	- Build images in a way that places any instructions that are likely to change towards the end of the file.
- Squash the image.
- Use no-install-recommends.
- Don't install from MSI packages for windows.
## Section 3: Containerizing an app - The commands
- `docker image build` is the command that reads a Dockerfile and containerizes an application.
	- The -t flag tags the image, and the -f flag lets you specify the name and location of the Dockerfile. With the -f flag, it is possible to use a Dockerfile with an arbitrary name and in an arbitrary location.
	- The build context is where your application files exist, and this can be a directory on your local Docker host or a remote Git repo.
- The FROM instruction in a Dockerfile specifies the base image for the new image you will build. It is usually the first instruction in a Dockerfile.
- The RUN instruction in a Dockerfile allows you to run commands inside the image which create new layers. Each RUN instruction creates a single new layer.
- The COPY instruction in a Dockerfile adds files into the image as a new layer. It is common to use the COPY instruction to copy your application code into an image.
- The EXPOSE instruction in a Dockerfile documents the network port that the application uses.
- The ENTRYPOINT instruction in a Dockerfile sets the default application to run when the image is started as a container.

