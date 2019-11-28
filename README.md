# the-magic-world-of-Kubernetes
One year study on kubernetes subject

# Terminology
- **bridge networks** : a link layer that forwards traffic between network segments . For docker , a bridge network allows containers connected to the same bridge to communicate while providing isolation from containers which are not connected to that bridge network.
- **NAT**: Network address Translation : NAT translates the IP addresses of computers in a local network in a single IP ADDRESS
- **egress traffic**:  traffic that exits an entity or a network
- **ingress traffic** : enters the boundary of a network
- **IPVS** : (IP Virtual Server) is built on top of the Netfilter and implements transport-layer load balancing as part of the Linux kernel.


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

  - **Multitenancy architecture** is an architecture in which a single instance of a software application serves multiple customers.Each customer is called a tenant. Tenants may be given the ability to customize some parts of the application, such as color of the user interface (UI) or business rules, but they cannot customize the application's code.
  ![Tenancy](assets/README-fa612.png)

- **Namespace**: an abstraction used in k8s to support multiple virtual clusters on the same physical cluster.

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
- **Container**: A container is a standard unit of software that packages up an application with all its dependencies so that the application runs quickly and reliably from one container to another .
`Containers don't make microservices,Containers change the level of encapsulation, of isolation from a virtual machine to a process`
benefits of containers (https://cloud.google.com/containers/)
- **Pod** : smallest component in a kubernetes cluster . it contains one or more containers and unique IP Address (Container of containers)
- **Node**: Known also as Minion is a Virtual or physical machine that contain all the necessary services to run pods (Container runtime, kubelet, kube-proxy).they are managed by the master (Pods Manager :oncoming_police_car:)
- **master (Cluster control plane)** : This is the Boss :fire: this component is a responsible for exposing the (API), scheduling the deployment and managing all the components of the cluster .

In the next section we will go into the details of each component:
## Container
I suppose that you have minimum requirement on this subject to work on kubernetes
### Container states
Once Pod is assigned to node by scheduler, kubelet starts creating containers using container runtime .
- Waiting : (default state) a container in this state is most likely doing its required operations like pulling images, applying secrets ..
- Running: :ok_hand: all good (postStart hook executed)
- Terminated : A container in this state is either successfully completed his duty OR failed for some reason .(preStop hook executed)
## pod
The smallest "unit of work " of kubernetes considered as **ephemeral**. Pods are one or more containers that share a network, namespace, and part of a single context.
a shared pod's context is a set of linux namespaces, cgroups  ...

containers within same pod share an IP address and port space they can find each other via localhost and can communicate with each other via staandard inter-process communication like POSIX ..
:grey_exclamation:Containers in different Pods can not communicate by IPC (inter-process communication ) without special config

Pods model is defined as application specific "logical host", it means that a pod contains one or more application containers which are relatively tighly coupled.
:exclamation:Pods are scaled up and down as a unit, all containers in a pod must scale together regardless their individual needs.it may lead to wasted ressources and expensive bills.
:

:question: Why not just run multiple programs in a single (Docker) container?
Using pods enhances transparency,decoupling software dependencies,ease to use and efficiency.
![pod_container](assets/README-fb157.png)
![pod_container](assets/README-f380a4dd.png)

### Pod status
- Pending : pod accepted by k8s system but one or more container has not been created
- Running :ok_hand: the pod is linked to a node, and all containers have been created. At least one Container is still running, OR is in the process of starting or restaring.
- Succeeded: All Containers in the Pod have terminated in success, and will not be restarted.
- Failed: All containers in the pod have terminated in success, and will not be restarted.
- Unknown: the state of the pod could not be obtained, typically due to an error in communicating withe host.

https://medium.com/faun/the-first-introduction-to-kubernetes-62d26f99caff

## Services
A service is an abstraction which defines a logical set of pods and a policy by which to access them  
Services are **persistant** objects used to reference ephemeral ressources.
for ensuring this persistance we can define :
  - static cluster IP
  - Static namespaced
  - DNS Name
We can define labels and selectors to
### Use case (frontend-backend)
if we have 3 replicas of our backend .3 pods -> 3 diffrent Ip address .which one we will choose to connect it to the frontend pod . In this case we can define a service which points on our 3 replicas and then poiints the frontend on the service .
### Types of services :
Every type of services is built in on top of another .
- **clusterIP** (default) service **only accepts** traffic from within the cluster.
![clusterIP](assets/README-14ebc.png)

- **nodePort**: service allows external traffic to be routed to the pods on a specific port you'll be able to contact pods via `<NodeIP>:<NodePort>`
Port can be mapped ether statically defined or dynamically defined (taken from a specific range defined by `--service-node-port-range` flag)
![nodeport](assets/README-7c4f2.png)

- **Loadbalancer**: exposes the service externally using a cloud provider's load balancer.LoadBalancer services extend NodePort

- **ExternalName**: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record. (CoreDNS 1.7 or higher is needed to use this ype)

📓Note : For exposing service to external traffic we usually use **ingress** as an entry point to the cluster .it lets you consolidate your routing rules into a single resource as it **can expose multiple services under the same IP**.

Todo: Selectors / Labels and namespaces
https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
## Node
The Kubernetes node has the necessary tools  to run application containers and be managed from the master systems.it's most likely a VM or physical machine .
![node archi](assets/README-d5a15.png)
![node archi2](assets/README-9dc7f.png)
### node component
#### Kubelet: :vertical_traffic_light: :blue_heart:
the most important controller in k8s .it's the primary implementer of the pod and node APIs that drive the container execution layer .kubelet is required on every host in the k8s cluster (even in the master .)he's always whatching kube-apiserver for any change (ensures that the containers described in those PodSpecs are running and healthy) . in addition, we can communicate to kubelet via HTTP endpoint, HTTP server or file.
 :space_invader: to verify -> Kubelet monitor containers with hooks, pod's policy, podSpec ..

#### Using Probes
To enhance monitoring feature,The kubelet can optionally perform and react to three kinds of probes on running Containers.
- Liveness probes : can be used to know when to restart a container , for example, Liveness probes could catch a deadlock : an application is running but unable to make progress . if the process in your container is **able to crash/become unhealthy on its own** whenever it encounters an issue , we don't need a liveness probe (pod's Restart policy can )  
- Readiness probes : can be used to know when a container is ready to **start accepting traffic** . this probe is usually used when the container needs to work on loading large data, configuration files or migrations during starup.
- Startup probes : used for containers with a slow start (exceeds default starttime `initialDelaySeconds + failureThreshold × periodSeconds`).this will avoid kubelet kill those containers before they are up and running.
kubelet three mechanisms for implementing those probes :

1) running a command inside a container
2) making an HTTP request against a container
3) opening a TCP socket against a container.

Probes are very powerfull tools to monitors containers but also very confusing if these probes are not carefully implemented . refer to : https://blog.colinbreck.com/kubernetes-liveness-and-readiness-probes-how-to-avoid-shooting-yourself-in-the-foot/
#### Container runtime:
is typically docker , used to manage containers in the node
#### kube-Proxy :
the network "plumber " for kubernetes services (manages the network rules in each node). enables in-cluster load-balacing and service discovery . three modes are available (ipvs, iptables)  


## Master
![master archi](assets/README-ec8ab.png)
# Cluster networking Basics
- Container-to-Container communications:
- pod-to-pod communications
- pod-to-service communications
- external-to-servioe communications
## services
### Cluster IP Service

we define a service with 10.0.165.39
1- A Pod in host C try to reach other pods, he will point out on the service ip address directly . Technically  the request hits host Iptables and it load-balances the connection between endpoints residing on the other hosts A and B. **Kubeproxy is responsible for updating iptables when a change occurs on the service (scaling up & sacaling down).**

![ClusterIP networking](assets/README-b2c49.png)
### nodePort Service
user can hit any host on nodeport IP and get to service even from external source
![nodePort networking](assets/README-03aa2.png)
### LoadBalancer Service
works with an external system to map a cluster external IP (provide cloud provider ) to the exposed port.

Traffic from the external load balancer is directed at the backend Pods. **The cloud provider decides how it is load balanced.**

![LoadBalancer service](assets/README-2f990.png)

>IPVS is introduced in k8s v1.8 . IPVS is more performant and opens the door to a wider feature set (port ranges, better lb rules etc) . Iptables is actually a bottle neck to scale clusters up to 5000 nodes.

# References
- https://www.bizety.com/2018/08/21/stateful-vs-stateless-architecture-overview/
- https://github.com/tkssharma/k8s-learning
- https://medium.com/tkssharma/kubernetes-architecture-quick-introduction-4ade0bd250d3
- https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/architecture.md
