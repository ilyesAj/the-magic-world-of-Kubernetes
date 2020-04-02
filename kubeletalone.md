# kubelet is a captain
In this markdown we will discuss how kubelet can manage pods even when we don't have an api-server,etcd or even the entire master-components .

Kubelet can manage pods bound to him on a specific node without the need of any external component, this is know as ``static Pods``

Based on manifest files stored in the filesystem of the node, kubelet will create the pods,monitor them and recreate them when they terminates or crashes .  

He will also checks periodically for a new pod manifest reads the files and creates them.

we can view instantiate pods in docker `docker ps`
## config
You can configure manifest folder with `--pod-manifest-path=` option in `kubelet.servie` or specify it in the config file under `staticPodPath`
## use-case: kubeadmin
kube-admin uses static pods when intializing a new cluster by adding api-server, etcd, .... manifests in manifest folder on master node. 
