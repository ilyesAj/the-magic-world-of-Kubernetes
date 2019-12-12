# Cluster networking Basics
- Container-to-Container communications:
- Pod-to-pod communications
- Pod-to-service communications
- External-to-service communications

https://kubernetes.io/docs/concepts/services-networking/ingress/
# Proxies :oncoming_police_car: :bus: :ambulance:

https://kubernetes.io/docs/concepts/cluster-administration/proxies/

There are several different proxies you may encounter when using Kubernetes:

## kube-proxy
- proxies UDP, TCP and SCTP
- does not understand HTTP
- provides load balancing
- is just used to reach services /  pods load-balancing
three modes (first one is userspace  deprecated 🔫  ) are available for kube-proxy:
### IPtables
iptables is a user-space application (no root needed) that allows configuring Linux kernel firewall (implemented on top of netfilter) by configuring chains and rules.

**:warning: Issues with IPTables as load balancer**
  - Latency to access service (routing latency)
  - Latency to add/remove rule

  IPTables are NOT incremental, for every manipulation, we have to copy all rules, make changes, save all rules back, note that IPTables are locked during rule update. When we have for exemple 20k services to implement with 8 rules/service (160k rules) we have to wait 5 hours to update tables :sleeping:

<!---
  - operates tables provided by linux firewall
  - manipulate packages at diffrent stage : pre-routing,post-routing,forward, input, output
  - do more operations : SNAT,DNAT, reject packets, port translation etc
--->

### IPvs
IPVS is a Linux kernel feature that is specifically designed for load balancing.
  - Transport layer load balancer which directs requests for TCP and UDP based services to real servers.
  - Same to IPTables, IPVS is built on top of Netfilter.
  - Support 3 load balancing mode: NAT, DR(L2 load balancing via MAC rewriting) and IP Tunneling

**:+1: Pros for using IPVS** :

  - Better performance (hash vs Chain(table))
  - More load balancing algorithm
      - Round robin, source/destination hashing
      - Based on latest load, least connection or locality, can assign weight to server
  - Support server health check and connection retry
  - Support sticky session

**:warning: pay attention to packet tracing**
packets that are handled by IPVS take a very different path through the iptables filter hooks** than packets under normal circumstances.  If you plan to use IPVS with other programs that use iptables then you will **need to research whether they will behave as expected together**. (Fear not though, Calico has been compatible with IPVS kube-proxy since way back when!)

### The kubectl proxy:

  - runs on a user’s desktop or in a pod
  - proxies from a localhost address to the Kubernetes apiserver
  - client to proxy uses HTTP
  - proxy to apiserver uses HTTPS
  - locates apiserver
  - adds authentication headers

#### Use case : Directly accessing the REST API with an http client (curl & wget ):

using kubectl proxy in this case is recommanded .it will do the most of the work for you :
  - Uses stored apiserver location
  - Verifies identity of apiserver self-signed cert
  - Authenticates to apiserver

Using kubectl proxy :

  ````sh
  kubectl proxy --port=8080
  curl http://localhost:8080/api/
  #expected
#  {
#  "versions": [
#    "v1"
#  ]
# }
  ````
### The Apiserver proxy:
is a bastion built into the apiserver that can :
- connect a user outside of the cluster to cluster IPs which otherwise might not be reachable
- runs in the apiserver processes
- used to reach a Node, Pod, or Service
- do load balancing when used to reach a Service

#### Protocols:
client -> proxy uses HTTPS (or http if apiserver so configured)

proxy -> target may use HTTP or HTTPS as chosen by proxy using available information
### The kube proxy:

- runs on each node
- proxies UDP, TCP and SCTP
- does not understand HTTP
- provides load balancing
- is just used to reach services

### (optional) A Proxy/Load-balancer in front of apiserver(s):

- existence and implementation varies from cluster to cluster (e.g. nginx)
- sits between all clients and one or more apiservers
- acts as load balancer if there are several apiservers.

### (optional) Cloud Load Balancers on external services:

- are provided by some cloud providers (e.g. AWS ELB, Google Cloud Load Balancer)
- are created automatically when the Kubernetes service has type LoadBalancer
- usually supports UDP/TCP only
- SCTP support (beta features on k8s) is up to the load balancer implementation of the cloud provider
- implementation varies by cloud provider.
# references

https://www.projectcalico.org/comparing-kube-proxy-modes-iptables-or-ipvs/
