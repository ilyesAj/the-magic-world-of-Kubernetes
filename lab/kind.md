For testing labs i was confronted to diffrent solutions to run a local k8s cluster :
# comparaison (Minikube VS k3s VS Kind)
- **Minikube** is a single VM running and simulates nodes with spawning process -> single node cluster
- **K3s** is a minified version of Kubernetes ->suitable for edge environments, can't test all potentiel of K8s
- **Kind** uses containers to simulate a kubernetes cluster -> :ballot_box_with_check: multi-cluster support, :ballot_box_with_check: multi-node support


|          | runtime                         | supported architectures                                                 | supported container runtimes   | memory requirements | requires root?                 | multi-cluster support | multi-node support | project page                  | Comment |
|----------|---------------------------------|-------------------------------------------------------------------------|--------------------------------|---------------------|--------------------------------|-----------------------|--------------------|-------------------------------|---------|
| minikube | VM                              | AMD64 ( for windows you have to install virtualbox to install minikube) | Docker,CRI-O,containerd,gvisor | 2GB                 | no                             | yes                   | no                 | https://minikube.sigs.k8s.io/ |         |
| kind     | container(kubernetes in docker) | Any machine that have docker installed                                  | Docker                         | 8GB                 | no                             | **yes**               | **yes**            | https://kind.sigs.k8s.io/     |         |
| k3s      | native                          | AMD64, ARMv7, ARM64                                                     | Docker, containerd             | 512 MB              | yes (rootless is experimental) | no                    | yes                | https://k3s.io/               |         |
# Kind
Kind is a tool for running local Kubernetes clusters using Docker containers .Nodes are optimized docker images to simulate VMs .The “node” image is a Docker image for running nested containers, systemd, and Kubernetes components.
kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

## install golang
````sh
wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
echo"export PATH=$PATH:/usr/local/go/bin" >> ~/.bash_profile
source ~/.bash_profile
````
to uninstall precedent versions of golang:
````sh
rm -rvf /usr/local/go/
````
to test golang installation:
````sh
cat << EOF >hello.go
package main

import "fmt"

func main() {
    fmt.Printf("Hello, World\n")
}
EOF
go build hello.go
./hello
# epectected output
# Hello, World
````

If GOPATH is set, binaries are installed to the bin subdirectory of the first directory in the GOPATH list. Otherwise, binaries are installed to the bin subdirectory of the default GOPATH ($HOME/go or %USERPROFILE%\go).
you have to add this path to $PATH in order to execute binaries fetched with go get
````sh
echo export PATH=$PATH:$PWD/go/bin >> ~/.bash_profile
source ~/.bash_profile
````
## install docker
https://docs.docker.com/install/linux/docker-ce/centos/
## install kind

````sh
GO111MODULE="on" go get sigs.k8s.io/kind
````
## Create first cluster
Kind uses yml files to create cluster . this is an example of yml i used to create my cluster
````yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  apiServerAddress: "10.153.1.124"
  apiServerPort: 6443
#  disableDefaultCNI: true
nodes:
- role: control-plane
- role: worker
- role: worker
# for HA
#nodes:
#- role: control-plane
#- role: control-plane
#- role: control-plane
#- role: worker
#- role: worker
#- role: worker
````
Refer to documentation for more details : https://kind.sigs.k8s.io/docs/user/configuration/

run the command below to create the cluster :
````sh
kind create cluster --config=config.yml --name=first -v 4 --kubeconfig=$PWD/kube.yml
````
By default, the cluster access configuration is stored in ${HOME}/.kube/config if $KUBECONFIG environment variable is not set.
to access to the cluster :
````sh
kubectl cluster-info --context kind-first --kubeconfig $PWD/kube.yml
````

# references
https://golang.org/doc/code.html#GOPATH
https://kind.sigs.k8s.io/
https://brennerm.github.io/posts/minikube-vs-kind-vs-k3s.html
