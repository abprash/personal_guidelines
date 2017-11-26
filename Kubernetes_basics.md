# Kubernetes Basics

## Full credits : [here](https://kubernetes.io/docs/tutorials/kubernetes-basics/cluster-intro/)
### What is Kubernetes?
* Kubernetes is an orchestration platform. 
* What is an orchestration application? Simple. In a microservice environment, the application is highly decoupled, therefore leading to a need for an orchestration agent. Kubernetes does that.
* Typically, Kube coordinates a highly available group of cluster of computers that are connected to work as a single unit. Take care, this cluster of machines can be physical or VMs too.
* Kubernetes automates the distribution and scheduling of applications containers across a cluster in a much more efficient way.
* A Kubernetes cluster consists of a master node and several other worker nodes.
* Communication between them is via the Kubernetes API which the master exposes. Since the API is exposed, programmers can also interact with it.
* To see how Kubernetes works on a small scale, we will be using MiniKube. Minikube creates a Kubernetes **cluster** on the current machine.
* So, each worker node may inturn have Docker or rkt to host and run the containers. 
* Thus, Kubernetes is an orchestration agent in a microservices environment and Docker is the local container appln.

### Commands to start playing with Minikube
* `minikube version` - just to see if minikube is installed.
* `minikube start` - will start a VM, which will have the Kubernetes cluster on it.
* `minikube stop` - will stop the kube VM.
* `kubectl version` - after going into the VM, we can use this command to see the Kube version on both client and server. Client here being the worker VM and server being the master VM.
* `kubectl get nodes` - we will be seeing that the node is ready to accept applications for deployment.


### Kubernetes Deployments
* Once we have a running cluster, we can then deploy containerized applications on top of it. 
* The "how" (ie how it deploys) part is taken care by the deployment configuration.
* The deployment instructor instructs Kubernetes on how to create and update instances in the cluster.
* Kubernetes Deployment Controller maintains the health of all nodes in the cluster and heals the cluster when one or more nodes go down.
* The applications need to be packaged into one of the supporting formats for Kubernetes to pick it up and deploy on its cluster.
* 