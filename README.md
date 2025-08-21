# kubernetes_architecture
kubernetes architecture in detail

Let me explain with Docker. Docker is a container platform where, without a container runtime, it doesn’t run (like Docker shim). It only has containers where applications run, but Docker alone doesn’t provide enterprise-level features.

Kubernetes came into the picture to support enterprise needs. It has advanced concepts like auto-healing, auto-scaling, and clustering. Within Kubernetes, we can create a master node and inside that, we create worker nodes. This forms the control plane and data plane.

The request from the user first goes to the master node and then to the worker node.
In Kubernetes, the smallest unit of deployment is a Pod. A Pod is like a wrapper over a container with advanced capabilities like shared network, shared storage, and easier communication.

Like in Docker, where containers are deployed, in Kubernetes Pods are deployed inside worker nodes. But here, the main responsible for running your Pods is not Docker Engine/shim—it’s handled by Kubelet + Container Runtime on each worker node.
	•	Kubelet: Ensures that containers described in your Pod spec are actually running in the worker node.
	•	Container Runtime: The low-level software (like Docker, containerd, CRI-O) that actually runs the containers.

In Docker, we have a default bridge network, which is mandatory for container communication. Unlike that, in Kubernetes we have Kube-proxy:
	•	Provides networking to every Pod,
	•	Assigns IP addresses,
	•	Offers load balancing capabilities.

Because of these, Kubernetes provides true enterprise-level support with auto-healing and auto-scaling.

For example, if you deploy an application using a YAML file, it will include all the necessary details: Pods, replicas, networking, volumes, etc.
If you specify two replicas of a Pod, Kubernetes automatically balances the traffic: some users are sent to Pod 1, and the rest to Pod 2. This traffic distribution is managed by Kube-proxy.
In the master node, there are:
	•	API server
	•	Scheduler
	•	Controller Manager
	•	Cloud Controller Manager
	•	etcd

A cluster is the specific standard in Kubernetes. In the cluster, there will be many nodes, so when a user creates a Pod, in which node should the Pod be assigned? There must be a core component in Kubernetes that handles these kinds of instructions.

When multiple users are accessing your cluster or even attempting malicious activity, there must be a component that takes all incoming requests. That component is the API server.
The API server exposes Kubernetes to the external world, while the data plane remains internal.

For example, let’s say Node 1 is free → the information first comes to the API server. But the API server only provides information, it doesn’t make decisions. This is where the Scheduler comes into the picture. It decides in which node the Pod should be placed based on available resources.

Once the Pod is scheduled, the information needs to be stored. For the backup of cluster information and safety, all cluster data is stored as objects or key-value pairs. This responsibility lies with etcd.

Controller Manager:
In Kubernetes, a ReplicaSet ensures the desired state of your Pods. For example, if there is only one Pod running, but the desired state requires two, the Controller Manager ensures another Pod is created to maintain the correct state.

Cloud Controller Manager:
This component extends Kubernetes into cloud platforms. It allows Kubernetes to integrate with providers like AWS (EKS), Azure (AKS), or Google Cloud (GKE).

![WhatsApp Image 2025-08-21 at 19 07 41_9898c0f0](https://github.com/user-attachments/assets/c95e8ccc-72ff-45f2-b3d8-92b470625dd2)



