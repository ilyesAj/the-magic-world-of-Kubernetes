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
iptables is a user-space application (no root needed) that allows configuring Linux kernel firewall(implemented on top of netfilter) by configuring chains and rules.

**:warning: issues with IPTables as load balancer**
  - Latency to access service (routing latency)
  - Latency to add/remove rule

  IPTables are NOT incremental, for every manipulation, we have to copy all rules, make changes, save all rules back, note that IPTables are locked during rule update. When we have for exemple 20k services to implement (160k rules) we have to wait 5 hours :sleeping:

<!---
  - operates tables provided by linux firewall
  - manipulate packages at diffrent stage : pre-routing,post-routing,forward, input, output
  - do more operations : SNAT,DNAT, reject packets, port translation etc
--->

### IPvs
IPVS is a Linux kernel feature that is specifically designed for load balancing. In IPVS mode, kube-proxy programs the IPVS load balancer instead of using iptables.  

This works, it also uses a mature kernel feature and IPVS is designed for load balancing lots of services; it has an optimized API and an optimized look-up routine rather than a list of sequential rules.  

One potential downside of IPVS is that **packets that are handled by IPVS take a very different path through the iptables filter hooks** than packets under normal circumstances.  If you plan to use IPVS with other programs that use iptables then you will **need to research whether they will behave as expected together**. (Fear not though, Calico has been compatible with IPVS kube-proxy since way back when!)

https://www.projectcalico.org/comparing-kube-proxy-modes-iptables-or-ipvs/
