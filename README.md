# the-magic-world-of-Kubernetes
One year study on kubernetes subject

# Terminology
- **bridge networks** : a link layer that forwards traffic between network segments . For docker , a bridge network allows containers connected to the same bridge to communicate while providing isolation from containers which are not connected to that bridge network.
- **NAT**: Network address Translation : NAT translates the IP addresses of computers in a local network in a single IP ADDRESS
- **RBAC**: (Role-Based Access Control) is an approch to restricting system access to authorized users .RBAC lets a user have access rights only to the information they need to .
- **stateless applications**: runs one function or service and does not require a persistant storage to work . all dataflow passed via a stateless service is typically transitory and the state is stored only in a separate a third-party back-end service like a database .Any associated storage is typically ephemeral. it ensures scalability and portability of the application .
- **stateful applications** : are typically databases .those applications processes requests based on the information relayed with each request and information stored from earlier requests .As a result , a stateful application must hold onto state information generated during the processing of the earlier request. [if you wonder how we can maintain state information ? ]( https://www.bizety.com/2018/08/21/stateful-vs-stateless-architecture-overview/)
- **Monolithic application**: describes a single-tiered software application in which the user interface and data access code are combined into a single program from a single platform.A modification made to a small section of code might require building and deploying an entirely new version of software.
- **batch processing**: a group of jobs, data or programs treated as one unit (traitement en lot in french)
- **legacy system**: outdated system but still being used .
- **Greenfield project**: A system developed in a total new environment:  new infra, new customers and even new owners.
- **CNI**: container network interface
- **CIDR**:(Classless inter-domain routing) also known as Supernets is a method for assigning IP addresses without using the standard IP address classes like Class A, Class B or Class C . For example, in the CIDR address 65.70.30.10/26, the /26 indicates the first 26 bits are used to identify the unique network leaving the remaining bits to identify the hosts. with CIDR , we have more control of the network, we can set a 50.25.11.2/17 mask (which whose forbiden in subnetting with classes) with a total of 32,768 IPs in range between 50.25.0.0 - 50.25.127.255.
-  **Microservices**: microservice architecture theory : 'Small autonomous services modeled around a business domain that work together.' by sam newman . amicroservice architecture is an architectural style that sy=tructures an application as a collection of services that are :
  - Loosely coupled : each component of the system has or make use of little or no knowledge of the the components of the system
![loosely coupled](assets/README-4b506.png)
  - Highly maintainable and testable
  - Independently deployable
  - Organized around business capabilities : Every single team can process an entire business capability . the opposite strategy would be to organize teams and servies arounds technology layers : if we take example of implementing the business capability of  "a reminder is sent by mail" feature .in a classic approch we have to involve UI team, DBA team, the Invoincing Team, the mail/communication team...
it amy lead to duplicated code BUT the cost of duplicating a bit of code is a lot lower than the cost of introducing tight coupling like sharing bib .
  - Owned by a small team

    [🚗 more on microservices topic](https://medium.com/citerus/whats-the-deal-with-this-microservices-thing-everyone-talks-about-fecc2883049a)
    [ 🎓 topic to study From Monolith to Microservices ](https://dzone.com/articles/from-monolith-to-microservices)
    - [ ] :construction_worker: try to migrate from a monolithic application to microservices application  [refer to] (https://medium.com/jeroen-rosenberg/from-monolith-to-microservice-architecture-on-kubernetes-part-1-the-api-gateway-eb82f8c2d10c )
    - 👷 [1st example] (https://github.com/ticket-monster-msa/monolith)
    - 👷 [2nd example] (https://github.com/micro-example/monolith-to-microservices)
    - 👷 [3rd example] (https://github.com/vmudigal/microservices-sample)
- **Serverless Architecture** : is a way of building applications without need to manage infrastructure . Developers will more focus on their code rather then the infra .   
![serverless](assets/README-10a8f.png)

  [🚗 more about Serverless topic](https://medium.com/swlh/serverless-architecture-complete-reference-guide-2019-55363c08d1be)
  >The essence of the **serverless** trend is the absence of the server concept **during software development**

  :question: is it cheaper than classic approach ? : it depends , Car analogy :
  ![cheaper ?](assets/README-9c5ab.png)

# The right Definition of Kubernetes

> We can't work on a technology if we can't clearly define it  !

Kubernetes is a production-grade, open-source infrastructre for the deployement, scaling, managemnet, and composition of *application containers* across clusters of hosts. Mainly defined as a **'Container Orchestrator'**, kubernetes provides container runtime, container orchestration, container-centric infrastructure orchestration, self-healing mechanisms such as health checking and re-scheduling, and service discovery and load balancing
Kubernetes aims to :
- Eliminate the burden( heavy load) of orchestrating physical/virtual compute, network, and storage infrastructure. This will enable developpers/application operatosto focus entirely on container-centric primitives for self-service operation.
- stable, portable foundation for building customized workflows and higher-level automation.

Kubernetes is based on the following *design ideals*:

- **Portable**: Runs everywhere (cloud,private, bare metal, home laptop) with consistent behavior so that applications are portable throughout the ecosystem as well as between development and production environments.
- **General-purpose**: kubernetes can run all mejors categories of workloads on a same infrastructure , stateless and stateful,services and batch, greenfield and legacy systems .
- **Meet users partway**: kubernetes adapts to the need of the user .it doesn't just supply a purely greenfield cloud-native applications but provides mechanisms to facilitate migration of monolithic and legacy applications.

- **Flexible**: kubernetes functionality can be consumed "à la carte" .you can even use your own solutions instead of build-in functionality.
- **Extensible**: kubernetes enables you to add additionnal capabilities to you environment.
- **Automatible**:The main goal of kubernetes is to reduce the burden of manual operations. By using declarative approch , the user defines the desired intent via API and kubernetes make it happen :rocket: . Imperative control is used to support high level orchestration and automation.
- **Advance the state of the art**: kubernetes is designed to advance the cloud native and Devops state of the art without forcing applications to depends on Kubernetes API .

# Kubernetes Architecture
We can define Kubernetes cluster with 5 main components :
![main architecture](assets/README-9c8cf.png)
- **Container**: A container is a standard unit of software that packages up an application with all its dependencies so that the application runs quickly and reliably from one container to another . benefits of containers (https://cloud.google.com/containers/)
- **Pod** : smallest component in a kubernetes cluster . it contains one or more containers and unique IP Address (Container of containers)
- **Node**: Known also as Minion is a Virtual or physical machine that contain all the necessary services to run pods (Container runtime, kubelet, kube-proxy).they are managed by the master (Pods Manager :oncoming_police_car:)
- **master (Cluster control plane)** : This is the Boss :fire: this component is a responsible for exposing the (API), scheduling the deployment and managing all the components of the cluster .

In the next section we will go into the details of each component:
## Container
I suppose that you have minimum requirement on this subject to work on kubernetes
## pod
The smallest "unit of work " of kubernetes considered as **Ephemral**. Pods are one or more containers that share a network, namespace, and part of a single context.
a shared pod's context is a set of linux namespaces, cgroups  ...

containers within same pod share an IP address and port space they can find each other via localhost and can communicate with each other via staandard inter-process communication like POSIX ..
:grey_exclamation:Containers in different Pods can not communicate by IPC (inter-process communication ) without special config

Pods model is defined as application specific "logical host", it means that a pod contains one or more application containers which are relatively tighly coupled.
:exclamation:Pods are scaled up and down as a unit, all containers in a pod must scale together regardless their individual needs.it may lead to wasted ressources and expensive bills.
:

:exclamation:

:question: Why not just run multiple programs in a single (Docker) container?

![pod_container](assets/README-fb157.png)
![pod_container](assets/README-f380a4dd.png)

https://medium.com/faun/the-first-introduction-to-kubernetes-62d26f99caff
## Node
![node archi](assets/README-d5a15.png)
## Master
![master archi](assets/README-ec8ab.png)
# Cluster networking Basics
- Container-to-Container communications:
- pod-to-pod communications
- pod-to-service communications
- external-to-servioe communications
# references
- https://www.bizety.com/2018/08/21/stateful-vs-stateless-architecture-overview/
- https://github.com/tkssharma/k8s-learning
- https://medium.com/tkssharma/kubernetes-architecture-quick-introduction-4ade0bd250d3
- https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/architecture.md
