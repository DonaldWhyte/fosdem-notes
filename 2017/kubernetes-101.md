### Kubernetes 101

Date: Sun 5th Feb
Time: 9:00
Speaker: Josh Berkus

Kubernetes -- OSS equivalent of Google's Borg

OpenShift -- RedHat's full stack offering container orchestration based on Kubernetes

Ghost -- NodeJs blogging framework. Apparently awesome and looks good.

For production -- explictly install SPECIFIC versions of required software. Don't just pull latest, or you're in for a world of hurt.

#### Pods

Pod contains 1+ containers that need to deploy together, inter-dependent.

The share same IP address and port space. And also share permanent storage.

Allow you to deploy one or more containers together.

You can define pods using YAML. List containers to run, ports they expose and some metadata.

#### Deploying Pods

Deploy to nodes. 

Nodes is a VM or bare metal machine.

Several nodes in a cluster.

Each node runs `kubelet`, the local kubernetes orchestration daemon.

#### Kubernetes API

You talk to Kubernetes using REST API.

There's also a CLI tool which calls the API, `kubectl`.

#### Test Instance of Kubernetes

`minikube` runs on local machine in VM. Useful for testing Kubernetes setup or demos.

#### Scaling Instances

Define a `Deployment` as a YAML file as well. The spec lists a bunch of pods, and how many replicates for each pod you want.

Kubernetes "scheduler" handles where they're supposed to go.

#### Sharing Services

Two types -- share services internally with each other. Or share services with the outside world.

Sharing a service in a container for **other** pods in the cluster. For example, sharing DB instances to service instances -- **internal** sharing.

Also sharing a service **externally** on the web. Often this means exposing Kubernetes to existing load balancers (e.g. run by clioud provider).

Again, define a `Service` as a YAML file. You can specify pods to expose and which port to expose them on. It will send requests to each pod instance in a round robin fashion.

Load balancing service type for exposing services to web.

#### Application Configuration

Config may vary based on versions, deployment environments(e.g. dev, staging, prod).

May have multi-tenant apps, passwords, performance settings, all with different config.

You can configure:

* ENV -- environment variables
* ConfigMap -- better way than env vars (define values as Kube object, which Kube makes available to containers either as ENV vars or files)
* Secrets -- you can pass hashed passwords, etc.

> For proper security, you should use an external secret store like Hashicorp
> Vault.

#### Stateful Services in Kubernetes

Two storage types:

Volumes -- local storage per node, temporary, recreate when new deploymnt
PersistentVolumes -- shared storage on network storage
    - needs to use network storage since it should survice the loss of the *node*, not just the container
StatefulSet -- need for replicated transacational databases
    - pod addressability
    - associate storage with specific pods
    - etc. 
    - see Josh's talk on handling Staeful apps in Kubernetes

#### How it Works

`etcd` -- distributed consensun store, holds metadata on distributed application. Stores all Kube state.

Create and edit Kube object using:

* writing directly to `etcd`
* `kubectl` + one-liners
* `kubectl` + YAML
* API + drivers (e.g. pyKube)

#### docker-compose vs. Kubernetes

docker-compose is aimed at developers, doesn't handle deployment.

Kubernetes handles deployment.

#### Autoscaling

CPU-based autoscaling -- use enough CPU on nodes, then new nodes are started. This is built-in to Kubernetes now, so easy(?) to configure.

#### Unanswered Questions

* How to add nodes to list of available resources to Kube?
* How does service sharing actually work?

