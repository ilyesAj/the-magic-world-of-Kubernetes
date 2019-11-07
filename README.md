# the-magic-world-of-Kubernetes
One year study on kubernetes subject

# Terminology
- **bridge networks** : a link layer that forwards traffic between network segments . For docker , a bridge network allows containers connected to the same bridge to communicate while providing isolation from containers which are not connected to that bridge network.
- **NAT**: Network address Translation : NAT translates the IP addresses of computers in a local network in a single IP ADDRESS
- **Pod** : smallest component in a kubernetes cluster . it contains one or more containers and unique IP Address (Container of containers)
- **Node**: Known also as Minion is a Virtual or physical machine that contain the services nessary to run pods and managed by the master components (Container runtime, kubelet, kube-proxy)
- **Container**: A container is a standard unit of software that packages up an application with all its sependencies so that the application runs quickely and reliably from one container to another . benefits of containers (https://cloud.google.com/containers/)
- **stateless applications**: runs one function or service and does not require a persistant storage to work . all dataflow passed via a stateless service is typically transitory and the state is stored only in a separate a third-party back-end service like a database .Any associated storage is typically ephemeral. it ensures scalability and portability of the application .
- **stateful applications** : are typically databases .those applications processes requests based on the information relayed with each request and information stored from earlier requests .As a result , a stateful application must hold onto state information generated during the processing of the earlier request. [if you wonder how we can maintain state information ? ]( https://www.bizety.com/2018/08/21/stateful-vs-stateless-architecture-overview/)
- **Monolithic application**: describes a single-tiered software application in which the user interface and data access code are combined into a single program from a single platform.A modification made to a small section of code might require building and deploying an entirely new version of software.
- **batch processing**: a group of jobs, data or programs treated as one unit (traitement en lot in french)
- **legacy system**: outdated system but still being used .
- **Greenfield project**: A system developed in a total new environment:  new infra, new customers and even new owners.
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

  :question: is it cheaper : it depends , Car analogy :
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

# Cluster networking Basics
- Container-to-Container communications:
- pod-to-pod communications
- pod-to-service communications
- external-to-servioe communications
# references
https://www.bizety.com/2018/08/21/stateful-vs-stateless-architecture-overview/
