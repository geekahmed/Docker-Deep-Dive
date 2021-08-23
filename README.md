
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
- [Chapter 5. The Docker Engine](#chapter5-the-docker-engine)
	- [1.-Docker Engine - The TLDR]()
	- [2.-Docker Engine - The Deep Dive]()
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

