# KUBERNETES:




First, first lets see the Docker vs Kubernetes. Docker is a container platform. Kubernetes is container orchestration platform.
Containers are in ephemeral in nature. (die and revive anytime). So, if there is docker then why we need to go for Kubernetes, let’s see the practical case now.

1st problem

On a single host, docker is installed and on top of the container, 100’s of containers are placed to run the application. Here containers are not related to each other. But if the first container takes lot of memory to run the application then the 100th of container may not receive enough memory to run the app, so it may either die or wont start the application. Indirectly, one container is killing the other one because as the containers sharing the same host.

The problem here is “**SINGLE HOST**”.

2nd Problem

For example, Unfortunately, one of the member in team killed the container. What happens now? we cannot access the application running inside the container right? So, user cannot check up on the containers every time manually whether container running or not by using the docker commands. In the real world, production environment had to deal up with 1000’s of containers which is quite impossible to manage without automation. 

There Kubernetes comes up with “**AUTO HEALING**” factor. Docker cannot auto heal the container , but Kubernetes does!

3rd problem 

Let’s say, docker is installed on top of EC2 machine and on top of docker, 100’s of containers are running, let’s say one of the container is “netflix application”, the capacity of the container is 4 CPU AND 4GB. It works normally until it reaches peak hours. for example, on weekends if there are lot of movies released on netflix, the traffic will be huge, so the container lacks the memory and space. 

How can we solve this, either by automation or manually. Docker cannot do both. 

By manually, if users increase from 1000 to 2000 then we create another container by configuring load balancer. usually we cannot access netflix through different IP address rights? Every user will search for (netflix.com). This can be done by load balancer either automatically or manually. Load balance will distribute the traffic equally depending up on the number of containers.

This is called “**AUTO-SCALING”**

Docker is a simple platform and does not provide Enterprise level support by default.

lets say you don’t anything about docker, what are the  minimal requirements to run the application on ec2 instance or any virtual  machine. Application should have

1. Load Balancer
2. Firewall 
3. Auto-Scale 
4. Auto- Healing
5. API Gateways 

As docker could not solve these problems, Kubernetes solve as it supports enterprise level support.

***How kubernetes solves these problems***

By default Kubernetes is a cluster, cluster is a group of nodes, if one of the container effects the another container, then immediately kubernetes will shift the effected container to the another node because its cluster in nature.

So, docker is installed on one single ec2 machine or virtual machine, but Kubernetes installs on master-node architecture.

Kubernetes can also install on single machine in the development environment. In the production environment it will install as a cluster.

The advantage of installing cluster is, as we discussed about the problem of single host in docker,

**Replication Controller(Replica Set):**

During the peak hours. if the number of users increases on a particular day then we can set the replica factor from 1 to 10 manually. kubernetes support only YAML files, there we can reset the replica factor. kubernetes also support HPA (Horizontally pod auto scaler) if there is huge load just increase by setting the threshold limit like if the threshold limit exceeds 80 % then set increase containers. this can be done by automatically. it can handle , if there is spike from 1000 to 1 lakh.

**Auto-healing,** 

Whenever there is damage, kubernetes should control or fix the damage.

If one of the containers goes down, then API server will notify the kubernetes and kubernetes will roll out a new container that user cannot know that something goes wrong. kubernetes will start a new container as it has auto healing feature. 

As docker does not support Enterprise level container orchestration platform, it cannot be used in production environment but docker swam does. 

There are some advanced load balance capabilities such as ingress controllers.

***K8’s Architecture:***


![arch](https://i0.wp.com/cloudwithease.com/wp-content/uploads/2022/09/what-is-kubernetes-dp.jpg?w=800&ssl=1)

Dockers contains containers, but how do they deploy it without container run time just like java and java run time. 

Kubernetes contains worker node and master node. For the worker node, the input comes from the master node.

**Worker node/Data plane** 

There are 3 services  such as 

1. **Kube-proxy:** it provides the network and IP address related to the application. It maintains network rules on the host and performs connection forwarding for Service IPs.
2. **Kubelet**: is the primary node agent that runs on each node in the cluster. main responsibility is to manage the containers running on the node and ensure that the containers described in the pod are running and healthy. If a pod fails to run correctly, the Kubelet communicates with the Kubernetes master to take necessary actions, such as restarting the pod.
3. **Container run time:** Kubernetes is designed to be container-runtime agnostic, meaning it can work with different container runtimes such as Docker, containerd, or CRI-O. The container runtime is responsible for pulling container images from a registry, creating container instances, managing their lifecycle, and providing isolation between containers. (execution environment)

**Master / Control plane:**

1. **API Server:** its the core component of the master node. lets say it talk about specific  instructions such as where the pod should be deployed like on which node, and if multiple users are trying to access/hacking the kubernetes or if we want to identify related stuff or security related stuff so on, there is should be one core component.
2. **Scheduler**: So if the user is trying to access node 1 through  API Server then API server will tells the user node 1 is free, but API server will schedule the pods on kubernetes.

who decides the information: API Server. who schedules the jobs: Schedule

1. **etcd:** Backing store of entire kubernetes. It stores the data in key-value pair. To restore the cluster info, etcd is the basic component.
2. **Controller manger:** Responsible for managing various controllers that trigger the state of the cluster. This includes controllers like ReplicaSet, Deployment, StatefulSet, DaemonSet, etc. Each controller watches the state of specific Kubernetes resources and takes action to ensure the desired state is maintained. The Controller Manager ensures these controllers are running and functioning properly.

**ReplicaSet:** A ReplicaSet is a Kubernetes controller that ensures a specified number of pod replicas are running at any given time. It helps maintain the desired number of identical pods to ensure high availability and scalability of applications. If a pod fails or is deleted, the ReplicaSet automatically replaces it to maintain the desired number of replicas.

3. **Cloud Controller manager (C-C-M):** It facilitates with the interactions between Kubernetes clusters and the underlying cloud provider's services. 

It Acts as an intermediary: CCM acts as a bridge between Kubernetes and the cloud provider's APIs. It translates abstract Kubernetes resource requests into cloud-specific actions understood by the cloud provider's APIs.

**Benefits**: Enables Kubernetes clusters to operate effectively on cloud infrastructure, abstracting away complexities of interacting with different cloud platforms and enhancing scalability, reliability, and flexibility.

In kubernetes , the lowest level of deployment is called pod. So, usually we deploy the application in containers either it is docker or kubernetes. But why pod is used in kubernetes instead of containers. Because when we run the containers in docker, we have to pass the commands in command interface, whereas in the the kubernetes all the command will be in the pod and in the yaml format. 

POD is nothing but definition of how to run a container!

Why kubernetes came with such complexation like pod? Because everything in kubernetes deals only with YAML files, as dockers has many commands to run one container and whereas in the kubernetes all the commands are in one place by wrapper of yaml files which causes advantage that wrapper can be as many time we want to. pod has cluster IP address but not the container inside [it.](http://it.here) Here kube-proxy will generate IP address here.

Pod can be single/multiple containers. For example if one container is running the application and wants to configure some properties from other container, now both can be deployed under the same pod. Due this shared pod there are some benefits such as

1. shared network
2. shared storage

Now lets talk about kube ctl, its a command line for kubernetes. Simply it will interact with kubernetes. 

**Difference between pod, container and deploy**

**Containers** :Basically, we run applications inside containers by passing Docker commands such as **`docker run -p`** like that, but if we want to test the container, like if it's healthy or not, again we should pass commands. This is a disadvantage here. 

**Pods:** So, instead of running the command every time, we put everything together in pod and wraps in a YAML file. and multiple containers can share the pod as the containers can share the storage and network. 

**Deployment:** if everything goes correctly with the pod, then why we need deployment? Well pod does not implement the auto scaling and auto healing. but deployment offers these features in kubernetes. 

- Deployments provide a higher-level abstraction for managing your application's lifecycle.
- They handle tasks like rolling updates, rollbacks, scaling, and self-healing.
- By using Deployments, you can ensure that your application remains available and responsive even when individual pods fail.

Do not create the pod directly, create it using deployment resources. deployment will have a replica set which is a controller in kubernetes. 

**Replica set:** Talks about High availability and load balancing. and it ensures that replica factor is always remain same. if the user deleted one pod from two pods, replica set will make sure the 2 replicas are there by creating a new one. this is done by deployment.

if there is small change in yaml file such as if the pods increase from 100 to 150 then the replica set makes sure that there will 150 of pods in the k8’s. 

And all these are managed by controllers parameter. Controllers always maintains the desired state is equal to actual state. controller is implementing auto healing feature.

**Controllers:**

- Controllers are responsible for maintaining the desired state of resources in Kubernetes.
- They continuously monitor the state of resources and take actions to ensure that the actual state matches the desired state.
- Controllers implement features like auto-scaling and auto-healing to keep your applications running smoothly.

***Kubernetes service***

We don’t deploy a pod, we usually deploy a deployment. 

why do we need a service in kubernetes and what happens if there is no service?

lets say there are 3 pods and concurrent users trying to access the application at the same time, if every request is going to only one particular pod then this pod will go down because it is getting too much of load so that's why, what we usually do is creating multiple replicas and the number of count of replicas depend upon the number of users trying to access your applications and also number of connections one particular pod can take.

so if somebody asks you what is the ideal pod size or what is the ideal pod count? it depends upon the number of concurrent users or the number of requests one replica of your pod can take.

so if application can handle 10 requests at one time and if you have 100 requests that are coming in then you have to create 10 parts. 

Another example : if there are three pods  let's say one of this pod has gone down for some reason there is some Network issue or in the world of kubernetes or in the world of containers a pod going down or a container going down is very common but what is the advantage of kubernetes is because of its Auto healing capability. Containers are ephemeral as the containers die they do not come up. similarly if a pod goes down it will not come up automatically unless you have the auto healing behavior that is implemented by the deployment in kubernetes or the replica set controller in kubernetes right

but the problem is let's say previously the IP addresses were 172.16.3.4 and  3.5 and 3.6. the IP address will change previously if it is 170.16.3.4 it might be 17.16.3.8, the IP address of the application has changed and now we are talking about the scenario where there is no Services Concept in kubernetes so what will happen is  application IP addresses you have to share with your test team or you have to share with your other project who is using this application , they'll try to access this application let's say there are three teams who are trying to access/test this application  what they said is, your application is not reachable or your application is not working. After debugging he is trying to send requests to 172.16.3.4 but the IP address of your application is 170 to 16 3.8

if you look at the real world, will never work like this let's say all of us use google.com on a day-to-day basis okay will Google ever tell you that try to access my application on a IP address called 100.64.2.7 .Google will never tell you that 1 million user access on this port the particular IP address another million people access on this specific IP address that doesn't work like that so what is the concept here the concept is Google does a load balancing.

There comes “**SERVICE concept**” 

1.  **Load Balancer**

Service (SVC) acts as a load balancer to send the traffic to multiple applications. 

Let’ say there are three teams who is accessing the application, but this time they access through  service which is on top of deployment instead of 3 IP address give to specific team. But the problem here is Like individual users, the load balancer relies on IP addresses to communicate with the backend instances. If the IP addresses change, the load balancer might try to send requests to the old IP addresses, resulting in communication failures. To overcome this challenge, we use service discovery mechanisms. Service discovery allows services to dynamically discover and communicate with each other without relying on fixed IP addresses.

2.  **Service Discovery**

Auto healing with IP address does not solve the real world problems, because whenever the application goes down, the IP address will change which causes downtime error. This problems is recognized by service called service discovery. 

So to get rid of this issue, service came with [Auto healing with Labels and selectors] the thing here is even when the application goes down for 100 times,  users don’t face any issue with application because in the deployment which is in yaml format manifests label and selectors instead of IP address. Lets took into an example to understand more about this. 

Using IP address:

team 1 will access 172.3.4.1

team 2 will access 172.3.4.4

team 3 will access 172.3.4.6

Application goes down, teams cannot access the application through IP address.

**Using Labels and Selectors:**

All the teams have same labels example: payment

1. Expose to world: In Kubernetes, exposing an application means making it accessible from outside the Kubernetes cluster, like accessing a website from the internet. For example, Google never tell you access me by this IP address.

**Three Service Types**: Kubernetes offers three ways to expose applications:

- **Cluster IP**: Makes the application accessible only within the Kubernetes cluster, useful for internal services.
- **Node Port**: Allows access to the application using the IP address of the Kubernetes nodes, suitable for internal users.
- **Load Balancer**: Provides a public IP address accessible from the internet, ideal for exposing applications globally.
1. **Example Scenarios**:
    - Cluster IP: Accessed only within the organization or network.
    - Node Port: Accessible by users with access to Kubernetes nodes or network.
    - Load Balancer: Accessible globally over the internet, like popular websites.
2. **Implementation Details**:
    - Cloud Control Manager generates public IP addresses for Load Balancer services.
    - Users can access Load Balancer services from anywhere on the internet.
    - Node Port services allow access to users within the organization or network.
    - Cluster IP services restrict access to users within the Kubernetes cluster.
3. **Application Examples**:
    - Load Balancer: Suitable for open-source applications or services like Amazon.com.
    - Node Port: Ideal for internal tools or services accessed by internal users.
    - Cluster IP: Used for services restricted to users within the Kubernetes cluster.

***Kubernetes Ingress***

when service is offering so many things such as load balancing, service discovery and exposing to the world why we need a tool like ingress then ?

Before 2015 Dec, there is no ingress concept. 

***Before Ingress :*** people used to create a deployment which would create a pod because creating a deployment we get Auto healing and auto scaling features and then you will create a service on top of it so that you can expose your application within your kubernetes cluster or outside the kubernetes cluster using the load balancer that is using the load balancer mode of service but there are some practical problems which people realized after using kubernetes.

***People migrated to Kubernetes:***
People started using kubernetes so obviously these users who migrated to kubernetes were migrating from the Legacy systems like people used to have virtual machines or physical servers on top of that they used to install their applications okay and what people used to do was they used a load balancer. Load balancers are Ngnix and fi.

These are Enterprise load balancers which offers good load balance and capability load balancing capabilities like for example you can do 

1. Ratio based load balancing that is we can say send three requests to pod number one seven request to pod number two.
2. Sticky sessions that means if one request is going to pod one then all the requests of that specific user have to go to pod one only this is called sticky sessions.
3. path based load balancing they can use domain or host based load balancing.
4. whitelisting that means only allow specific customers to access the application.
5. blacklisting that means customers are like hackers do not allow any users coming from Pakistan we can Define the traffic.

These are the capabilities that Enterprise load balancers support.

***Problem arises:***

**Problem 1:**

Now the problem here was people who used VM’S applications and when they migrated from this to kubernetes. Initially people were happy that kubernetes was offering Auto healing and Auto scaling and service Discovery and also exposing the applications to external world. But off late they realized that we are using doing same things what we used to do in vm’s but here service was providing load balancing mechanism but the load balancing mechanism the service was providing is a simple round Robin load balancing.

Round Robin load balancing is simple to implement and works well in scenarios where servers have similar capabilities and handle similar workloads. However, it may not be suitable for all situations, especially if servers have different capacities or if certain requests require specific handling. In such cases, more sophisticated load balancing algorithms may be used.

So now people were unhappy as they were getting more advanced enterprises load balance capabilities such as TLS , WAF  and there are more in legacy systems.

**Problem 2:**

Exposing the applications to external World using load balancer mode service can create the service as load balancer mode but what is the problem.

let's say you have 100 Services if you take companies like Amazon they have some thousands of services each of the service when they create ,the service acts as type load balancer mode and the cloud provider was charging them for each and every IP address because these are static public IP address. so if there are thousands of micro services or if there are thousands of services that you require for your applications on kubernetes so cloud provider was charging very heavy.
But on virtual servers people used to create one load balancer whether you have one application two application three applications so they used to configure in their load balancer ,if the request is coming to [amazon.com](http://amazon.com/)/ABC send request to app one if it is coming to /XYZ go to app2 and they
used to only expose these load balancer with static public IP address so what is happening is here they just have one IP address which they are getting from the cloud provider or even within their organization.

**The problems are solved by Kubernetes Ingress:**

Red Hat OpenShift is a popular enterprise Kubernetes platform developed by Red Hat. It builds on top of Kubernetes, adding additional features and functionality to make it more suitable for enterprise environments

People complained about k8’s saying that when we were on Virtual machines we were enjoying all the good capabilities of load balancers and because of which our  applications were very secure and we had reduced costs but once we moved to kubernetes we realized that this is a very big problem so kubernetes people have also agreed to it.

So kubernetes came up with a solution: says that we will allow the users of kubernetes to create a resource called Ingress Resource and there are lots of enterprise load balancers are there which need to be implemented on top of k8’s and this implementation should done by the users. 

Example : if u need path based routing on kubernetes which are very heavily using on virtual machines. On kubernetes cluster create a Ingress resource and then you create a Ingress controller.
Example: Create one yaml file and inside the YAML file say that I want Pathways routing. but who will implement this ,the load balancer you want to use . but there are hundreds of load balancers in the market so what kubernetes said is we cannot support all of you and cannot create the logic for all in the kubernetes master or the API server instead you people create something called as Ingress controller.
so let's say that you want to create specific capability using nginx load balancer so the nginx company will write a nginx Ingress controller and kubernetes users on this kubernetes cluster will deploy the Ingress controller using Helm charts and  yaml manifest. Ingress controller will watch for the Ingress resource and it will provide you the path based routing .

what is ingress control: it is just a load balancer sometimes it can be a load balancer plus API Gateway as well. API Gateway offers some additional capabilities.

1. **Pre-Kubernetes Setup with Nginx**: In traditional virtual machine setups, if you were using Nginx as your load balancer, you would typically find the necessary resources and documentation on the Nginx GitHub page. Configuration of the Nginx controller and its routing rules is usually done manually or through scripts on the VMs themselves
2. **Deploying Nginx Ingress Controller onto Kubernetes**: Before migrating to Kubernetes, you would deploy the Nginx Ingress Controller onto your Kubernetes cluster. This can be done by following the instructions provided by Nginx, usually available on their GitHub page or other documentation sources. Configuration of the Nginx controller and its routing rules is defined declaratively using Kubernetes manifests, such as Ingress resources, allowing for automated and scalable traffic management
3. **Creating Ingress Resources**: Once the Nginx Ingress Controller is deployed, you can create Ingress resources within Kubernetes. These resources define routing rules for incoming traffic. Depending on your requirements, you can create different types of Ingress resources, such as path-based, TLS-based, or host-based routing.
4. **One-Time Activity**: The decision of which Ingress controller to use and which load balancer to deploy is a one-time activity for DevOps engineers. They need to choose the appropriate solution based on their organization's needs and preferences. Once the Ingress controller is deployed, it can handle routing for any number of services within the Kubernetes cluster.

**Configs and secrets:**

In Kubernetes, when we're dealing with applications that require certain configuration data like database connection details, it's crucial not to hardcode this information directly into the application. Instead, Kubernetes offers two main resources to handle this: ConfigMaps and Secrets.

1. **ConfigMaps**: These are used to store non-sensitive configuration data, such as database ports, connection types, or any other environment-specific details. As an application developer or DevOps engineer, you can create a ConfigMap in Kubernetes and populate it with the required information. This information can then be accessed by pods within the cluster either as environment variables or as files mounted inside the pod's filesystem.
2. **Secrets**: On the other hand, Secrets are specifically designed to handle sensitive data like database passwords or API keys. Kubernetes ensures the security of this data by encrypting it at rest before storing it in the cluster's etcd database. Additionally, access to Secrets can be tightly controlled using Role-Based Access Control (RBAC), ensuring that only authorized users or components have access to this sensitive information.
- Kubernetes can encrypt data stored in etcd, the cluster's key-value store, using encryption mechanisms like encryption at rest (e.g., through the use of encryption keys managed by the cluster's encryption provider).

Kubernetes also follows the principle of least privilege dictates that users, processes, or systems should only have the minimum levels of access or permissions required to perform their necessary tasks. 

If you have configuration data that frequently changes and updating environment variables inside containers isn't feasible because containers don't allow dynamic changes to environment variables without restarting, Kubernetes suggests using volume mounts instead.

With volume mounts, you can mount the ConfigMap data as files inside your pod's filesystem. This allows developers to read the information from files rather than environment variables, providing a more flexible approach to handling dynamic configuration.

**RBAC( Role Based Access Control)**: related to security.

divided into 2 parts

1. users management
2. service account

### **Ensuring Security with Role-Based Access Control (RBAC)**

Imagine a scenario where a team member accidentally deletes critical data related to etcd in your Kubernetes cluster. It could cause chaos if there's no way to revert to the original state. This is where Role-Based Access Control (RBAC) comes in.

### **What is RBAC?**

RBAC allows you to control access to resources based on the roles of individual users within an organization. It defines who can do what within your Kubernetes cluster, depending on their assigned role.

### **Managing User Access**

Just like managing users, RBAC helps manage access for applications and services running on the cluster. It ensures that only authorized actions can be performed by each user or service.

### **Implementation in Kubernetes**

Kubernetes provides RBAC capabilities through its API server. You can configure RBAC settings by passing certain flags to the API server.

### **Offloading User Management**

Similar to logging in with social media accounts like Instagram or Google, Kubernetes can offload user management to external identity providers. For instance, if you're using Amazon EKS, you can utilize IAM users to log into Kubernetes.

### **Conclusion**

RBAC plays a crucial role in ensuring security and access control within Kubernetes. By defining roles and permissions, you can prevent unauthorized actions and safeguard your cluster's integrity.

***Managing User Access in Kubernetes with External Identity Providers***

1. **Setting Up Access**: To let people log into a Kubernetes cluster, you create something called an IAM provider or IAM authentication provider.
2. **User and Group Management**: You've probably already set up users and groups in IAM. When someone logs into Kubernetes, they use the same username and group they have in IAM.
3. **External Identity Providers**: Kubernetes can use different identity providers like LDAP, Okta, or SSO. You decide which one to use and set it up accordingly.
4. **Identity Brokers**: You can also use brokers like Keycloak to manage user access. It's a popular choice for connecting Kubernetes with systems like GitHub, where you manage users and permissions.
5. **Flexibility**: With Keycloak, for example, you can link it to your GitHub account. This means changes you make in GitHub (like adding users or changing permissions) automatically apply to Kubernetes.
6. **Conclusion**: Kubernetes relies on external systems to manage user access. You configure these systems to control who can log in and what they can do in Kubernetes.

***"Managing Access Control in Kubernetes with Service Accounts, Roles, and Role Bindings”***

1. **Service Accounts**: Think of a service account like a configuration file (YAML) for a specific task. It's similar to creating a YAML file for a pod. In this file, you specify details like API version, kind, and name.
2. **Default Service Accounts**: When you run a pod, Kubernetes automatically assigns a default service account to it. This service account allows your applications to communicate with the Kubernetes API server to access resources.
3. **Custom Service Accounts**: You can create your own service accounts if needed. Kubernetes will then use these custom service accounts for your pods instead of the default one.
4. **Role and Role Binding**: After setting up service accounts, you need to define who can access what within Kubernetes. Kubernetes uses two key resources for this: roles and role bindings.
5. **Roles**: Roles define permissions. For example, you might create a role that grants access to pods, config maps, and secrets within a specific namespace. If the access needs to be across the entire cluster, you'd create a cluster role instead.
6. **Role Binding**: Once you've defined roles, you need to assign them to users or service accounts. This is done through role bindings. It's like saying, "This role applies to these users or service accounts."
7. **Conclusion**: Roles and role bindings are crucial for managing who can do what within Kubernetes. Roles specify what actions are allowed, and role bindings associate those roles with specific users or service accounts.

**Kubernetes monitoring with** **Prometheus and Grafana**

1. **Single vs. Multiple Clusters**: Managing a single Kubernetes cluster may be feasible for a single DevOps engineer. However, when multiple teams use the same cluster, issues such as deployment failures or service disruptions may arise, requiring efficient monitoring and troubleshooting.
2. **Importance of Observability**: As the number of Kubernetes clusters increases, the need for observability becomes crucial. Observability allows DevOps teams to monitor the health and performance of clusters, detect anomalies, and troubleshoot issues effectively.
3. **Prometheus and Grafana**: Prometheus is an open-source monitoring and alerting toolkit initially developed by SoundCloud. It collects metrics from Kubernetes clusters, stores them in a time-series database, and offers querying capabilities. Grafana complements Prometheus by providing visualization and dashboarding features, enabling users to create custom dashboards for monitoring Kubernetes clusters.
4. **Prometheus Architecture**: The architecture of Prometheus includes a Prometheus server responsible for collecting metrics from Kubernetes clusters via the API server. Prometheus stores these metrics in a time-series database on disk and can be configured with an alert manager to send notifications in case of anomalies or failures. Users can interact with Prometheus through its UI, execute Prometheus queries (PromQL), or integrate it with other tools via APIs.

1. **Role of Grafana**: Grafana serves as a visualization layer for Prometheus, allowing users to create visually appealing dashboards to monitor Kubernetes clusters' performance and health. Grafana retrieves data from Prometheus

**Custom Resource and Definitions:**

**1. Custom Resource Definitions (CRDs):**

- CRDs allow users to extend the Kubernetes API by defining their own custom resources.
- Think of CRDs as blueprints for creating custom resources. They specify the structure and behavior of the custom resources.
- When you want to introduce a new resource to Kubernetes, such as Istio's VirtualService or Argo CD's Application, you define it using a CRD.
- CRDs are YAML files that define the schema for your custom resources, including the API version, kind, metadata, and specifications.

**2. Custom Resources (CRs):**

- Custom resources are instances of the custom resources defined by CRDs.
- Once you've defined a CRD, users can create instances of it, just like they create standard Kubernetes resources like Deployments or Services.
- For example, if you define a CRD for Istio's VirtualService, users can create VirtualService custom resources to configure Istio features like routing rules.
- Custom resources are submitted to the Kubernetes API server just like native Kubernetes resources.
- These instances, known as custom resources, enable users to utilize new features or functionalities within Kubernetes that are not available by default

**3. Custom Controllers:**

- Custom controllers are controllers specifically tailored to watch and manage custom resources.
- When a custom resource is created, updated, or deleted, the custom controller takes action based on the desired state defined in the custom resource.
- Custom controllers are responsible for reconciling the desired state of custom resources with the actual state of the cluster.
- They can perform various tasks, such as deploying pods, configuring services, or updating configurations, based on the specifications provided in the custom resources.

**Overall Workflow:**

- DevOps engineers deploy CRDs to extend the Kubernetes API with custom resources.
- Users create custom resources based on the defined CRDs to utilize new features or functionalities within Kubernetes.
- Custom controllers, deployed by DevOps engineers, watch for custom resources and ensure that the desired state defined in the custom resources is reflected in the cluster's actual state.

In summary, CRDs define the structure of custom resources, custom resources are instances of these definitions, and custom controllers manage the lifecycle and behavior of these custom resources within Kubernetes clusters.
