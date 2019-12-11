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
two modes are available for kube-proxy:
### IPtables
iptables is a Linux kernel feature that was designed to be an efficient firewall with sufficient flexibility to handle a wide variety of common packet manipulation and filtering needs.  It allows flexible sequences of rules to be attached to various hooks in the kernel’s packet processing pipeline.

In iptables mode, kube-proxy attaches rules to the “NAT pre-routing” hook to implement its NAT and load balancing functions. This works, it’s simple, it uses a mature kernel feature, and, it “plays nice” with other programs that also work with iptables for filtering (such as Calico!).

However, the way kube-proxy programs the iptables rules means that it is nominally an O(n) style algorithm, where n grows roughly in proportion to your cluster size (or more precisely the number of services and number of backend pods behind each service).
### IPvs
IPVS is a Linux kernel feature that is specifically designed for load balancing. In IPVS mode, kube-proxy programs the IPVS load balancer instead of using iptables.  

This works, it also uses a mature kernel feature and IPVS is designed for load balancing lots of services; it has an optimized API and an optimized look-up routine rather than a list of sequential rules.  

One potential downside of IPVS is that **packets that are handled by IPVS take a very different path through the iptables filter hooks** than packets under normal circumstances.  If you plan to use IPVS with other programs that use iptables then you will **need to research whether they will behave as expected together**. (Fear not though, Calico has been compatible with IPVS kube-proxy since way back when!)

https://www.projectcalico.org/comparing-kube-proxy-modes-iptables-or-ipvs/
