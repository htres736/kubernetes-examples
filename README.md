Step by step commands google doc:
https://docs.google.com/document/d/1oW0sPjLoBvcv1V7_YBxlbXiCGm_0PW7I8Xk3ltJjSog/edit#heading=h.29pgcopranu

AKS google doc:
https://docs.google.com/document/d/1KtnlBIjmyf7XhLB642N6nnglHGBEyg4swbqALCu0f6Y/edit

Google drive with all docs:
https://drive.google.com/drive/u/0/folders/1rPaJjuPJCl4_uU8guvk2ba_NlRNAs8L7

# Initialize cluster

Note: All commands to be run as root ( or with sudo)

On Master


```
kubeadm init --node-name master
```


Output:


```
root@ip-172-31-19-105:~# kubeadm init --node-name master
I1002 14:49:51.860725   12621 version.go:254] remote version is much newer: v1.22.2; falling back to: stable-1.20
[init] Using Kubernetes version: v1.20.11
[preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
        [WARNING SystemVerification]: this Docker version is not on the list of validated versions: 20.10.5. Latest validated version: 19.03
        [WARNING Hostname]: hostname "master" could not be reached
        [WARNING Hostname]: hostname "master": lookup master on 127.0.0.53:53: server misbehaving
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local master] and IPs [10.96.0.1 172.31.19.105]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [localhost master] and IPs [172.31.19.105 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [localhost master] and IPs [172.31.19.105 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.
[apiclient] All control plane components are healthy after 69.002216 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.20" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node master as control-plane by adding the labels "node-role.kubernetes.io/master=''" and "node-role.kubernetes.io/control-plane='' (deprecated)"
[mark-control-plane] Marking the node master as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: 219bkb.md2830ygm2ydyrh2
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.19.105:6443 --token 219bkb.md2830ygm2ydyrh2 \
    --discovery-token-ca-cert-hash sha256:6fc037aaff7794d0b28a8796a03f87a6cad6ac0b965402524c50c4a57e91efc8 

```




Add kube config file

  On Master


```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```



# Install CNI

On Master


```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```



# Optional (auto completion)

On Master


```
echo 'source <(kubectl completion bash)' >>~/.bashrc
source .bashrc 
```



# Add Nodes

<span style="text-decoration:underline;">On Both Nodes</span>, execute below command:


```
kubeadm join <IP Address>:6443 --token <token> \
    --discovery-token-ca-cert-hash <some hash>
```



# Verify the cluster setup:


```
kubectl get nodes
```


Output


```
root@ip-172-31-19-105:~# kubectl get nodes
NAME               STATUS   ROLES                  AGE     VERSION
ip-172-31-19-129   Ready    <none>                 3m21s   v1.20.5
ip-172-31-20-26    Ready    <none>                 3m28s   v1.20.5
master             Ready    control-plane,master   10m     v1.20.5
```



# Pods


## Create a pod


```
kubectl run myfirstpod --image=httpd --port 80
kubectl get pods -o wide
```



## Create a pod Declaratively:

pod.yaml 


```
apiVersion: v1
kind: Pod
metadata:
        name: mypod
        labels:
                env: prod
spec:
        containers:
                - image: nginx
                  name: mynginxcontainer
                  ports:
                          - containerPort: 80
```



```
kubectl apply -f pod.yaml
```



# Describe a resource


```
kubectl describe <resource type> <resource name>
kubectl describe nodes master 
```



# Namespace


## Create namespace


### Option 1: Imperatively


```
kubectl create namespace mynamespace
```



### Options 2: Declaratively


```
apiVersion: v1
kind: Namespace
metadata:
  name: mynamespace
```



## Create Pod in a namespace


```
root@ip-172-31-19-105:~/demo/pod# cat namespace-pod.yaml 
apiVersion: v1                  # version of api-resource
kind: Pod                       # api-resourse 
metadata:                       # information of the api-resource
  name: namespaced-pod
  namespace: mynamespace
spec:                            # configuration of the api-resource
  containers:
  - name: nginxcontainer
    image: nginx

```



## List the pods in a namespace


```
kubectl get pod -n mynamespace 
```



## Delete Namespace


```
kubectl delete namespaces mynamespace
```



# Generate YAML template

To generate YAML template from imperative command


```
kubectl run podname --image nginx --port 80 --dry-run -o yaml > pod.yaml
```



# Labels and Selectors


# Labels


## Apply Labels


```
kubectl label nodes <one of the nodes' name> environment=production location=usa
kubectl label nodes <the other nodes' name> environment=production location=india
kubectl label nodes <master nodes' name> environment=test location=usa
```



## Get nodes with label information


```
kubectl get nodes --show-labels
```



## Delete a label


```
kubectl label node master environment-
```



## Update a label


```
kubectl label node master --overwrite location=usa
```



# Selector

Select all the nodes with environment set to production


```
kubectl get nodes -l environment=production
```



# 


# Scheduling


# nodeName


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    app: myapp
  name: pod
spec:
  nodeName: ip-172-31-20-26  #Desired Node Name
  containers:
  - image: nginx
    name: pod
    ports:
    - containerPort: 80
```



# nodeSelector


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: node-selector-pod
spec:
  nodeSelector:
     environment: production  # Node labels
  containers:
  - image: nginx
    name: pod
    ports:
    - containerPort: 80
```



# Affinity


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: affinity-pod
spec:
  containers:
  - image: nginx
    name: pod
    ports:
    - containerPort: 80
  affinity: 
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: environment
            operator: In
            values: 
            - production
            - test
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        preference: 
          matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
```



# Taints and Tolerations


## Effects:



1. NoSchedule
2. PreferNoSchedule
3. NoExecute


## Taint a node:


```
kubectl taint node ip-172-31-19-129 type=gpu:NoSchedule
```



## Tolerate the taint in a Pod


```
apiVersion: v1                  
kind: Pod                       
metadata:                       
  name: test-taint-pod
spec:                           
  containers:
  - name: nginxcontainer
    image: nginx 
  tolerations: 
  - key: type
    operator: Equal
    value: gpu
    effect: NoSchedule
```



## Untaint a node:


```
kubectl taint node ip-172-31-19-129 type=gpu:NoSchedule-
```



# Multi Container Pod


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: multi-container
spec:
  containers:
  - image: nginx
    name: nginx
    ports:
    - containerPort: 80
  - name: tomcat
    image: tomcat
    ports:
    - containerPort: 8080
```


Verify that pod is running


```
kubectl get pods -o wide
```


Access applications inside the pods:


```
curl <pod-IP>
curl <pod-IP>:8080
```



# Logs 

Print logs of specific containers in a pod:


```
kubectl logs [-f] <Podname> [containername]
kubectl logs -f multi-container podname
```


Print logs of all containers in a Pod


```
kubectl logs -f --all-containers multi-container
```



# Customization


# Environment variables

Create a pod based on httpd image, with the below environment variables:



1. NAME=”yourname”
2. LOCATION=”yourcountry”

    ```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mysql
  name: mysql
spec:
  containers:
  - env:
    - name: NAME
      value: "yourname"
    - name: Location
      value: "yourCountry"
    image: httpd
    imagePullPolicy: Always
    name: custom-env-pod
```




# Custom Commands


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: customcommand
spec:
  containers:
  - image: alpine 
    name: alpine
    command: ['sh','-c','echo "Hello Kubernetes" && sleep 100']
```



# 


# Resource Limits

Pods with resource limits:


```
apiVersion: v1
kind: Pod
metadata:
  name: resource-limit
spec: 
  containers: 
  - image: nginx
    name: nginx
    resources:
      requests: 
        cpu: 0.5
      limits:
        cpu: 1
```



# Controllers


# ReplicaSet


```
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: my-rs
spec: 
  replicas: 3
  selector:
    matchLabels: 
      app: simplilearn
  template: 
    metadata: 
      labels: 
        app: simplilearn
    spec:
      containers:
      - name: nginx
        image: nginx
```


Check the running replicaset:


```
kubectl get rs        
```


Check the pods managed by the ReplicaSet (should result in 3 pods):


```
kubectl get pods -l app=simplilearn
```


Change the scale of the replicaset


```
kubectl scale replicaset my-rs --replicas=5
```


Check the pods managed by the ReplicaSet (should result in 5 pods):


```
kubectl get pods -l app=simplilearn
```





# Deployment


```
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: my-dep
spec: 
  replicas: 10
  selector:
    matchLabels: 
      app: sl
  template: 
    metadata: 
      labels: 
        app: sl
    spec:
      containers:
      - name: nginx
        image: nginx:1.19
```


List the deployment:


```
kubectl get deployments
```


List pods in the deployment:


```
kubectl get pod
```


List the replicasets (which are part of deployment)


```
kubectl get rs
```


Get details of a Deployment


```
kubectl describe deployments.apps my-dep 
```


Scale a deployment


```
kubectl scale deployment my-dep --replicas=5
```


Rollout a new version


```
kubectl set image deployment my-dep nginx=nginx:1.20 --record
```


Check the rollout history


```
kubectl rollout history deployment my-dep 
```


Rollout another new version


```
kubectl set image deployment my-dep nginx=nginx:1.21 --record
```


Check the rollout history


```
kubectl rollout history deployment my-dep 
```


Rollback to a specific version


```
kubectl rollout undo deployment my-dep --to-revision 1
```



# DaemonSet


```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-ds
spec:
  template:
    metadata:
      name: pod-for-ds
      labels:
        app: ds
    spec:
      containers:
      - image: nginx
        name: nginx
  selector:
    matchLabels:
      app: ds
```


List Daemonsets


```
kubectl get ds
```


Describe Daemonset


```
kubectl describe daemonsets.apps my-ds 
```


Check the pods in the daemonset


```
kubectl get pods -o wide
```



# Services


# ClusterIP 


```
kubectl expose deployment my-dep --name my-svc --port 80
```



# NodePort 


```
kubectl expose deployment my-dep --name my-nodeport-svc --port 80 --type NodePort
```



# LoadBalancer 


```
kubectl expose deployment my-dep --name my-lb-svc --port 80 --type LoadBalancer
```


List services


```
kubectl get svc
```



# 


# Jobs


```
apiVersion: batch/v1
kind: Job
metadata: 
 name: print-hello
spec: 
 completions: 5
 template: 
   spec:
     containers: 
     - name: print-hello
       image: alpine
       command: ['echo','hello']
     restartPolicy: Never
```



# CronJobs


```
apiVersion: batch/v1beta1
kind: CronJob
metadata: 
  name: cj-cimplilearn
spec: 
  schedule: "* * * * *"
  jobTemplate:
    spec: 
      template: 
        spec:
          containers:
          - name: print-simplilearn
            image: alpine
            command: ['echo','hello','simplilearn']
          restartPolicy: Never
```



# 


# Secrets


# Manage Secrets

Create a secret


```
kubectl create secret generic mysecret --from-literal password=simplilearn --from-literal username=db_user
```


List all secrets


```
kubectl get secrets mysecret
```


Details about a secret


```
kubectl describe secrets mysecret
```


Get the encoded data stored in secret


```
kubectl get secrets mysecret -o yaml
```


Decode the retrieved data


```
echo c2Vuc2l0aXZlcGFzc3dvcmQ= | base64 -d
```



# Use Secret in Pod


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mysql
  name: mysql
spec:
  containers:
  - env:
    - name: MYSQL_ROOT_PASSWORD
      value: rootpassword
    - name: MYSQL_DATABASE
      value: simplilearn
    - name: MYSQL_PASSWORD
      valueFrom:
        secretKeyRef: 
         name: mysecret
         key: password
    image: mysql
    name: mysql
    ports:
    - containerPort: 3306

```



# 


# ConfigMap

Create a file called `max_packets.cnf` with following content:


```
[mysqld]
max_allowed_packet = 100M
```


Create configmap to store the contents of the above file


```
kubectl create configmap mysql-config --from-file max_packets.cnf
```


Describe the configmap


```
kubectl describe configmaps mysql-config
```



# Exec: Connect to container


```
kubectl exec my-dep-5cd7595d84-47qxj -- printenv
kubectl exec -it my-dep-5cd7595d84-47qxj -- /bin/bash
```



# Volume Storage


# hostPath


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: storage-pod
  name: storage-pod
spec:
  containers:
  - image: alpine
    name: storage-pod
    command: ["/bin/sh", "-c","shuf -n 1 -i 0-1000 >> /opt/number.out && sleep 100"]
    volumeMounts: 
    - name: number-storage
      mountPath: /opt
  volumes:
  - name: number-storage
    hostPath:
      path: /tmp/data
      type: DirectoryOrCreate
```



# 


# ConfigMap

Create `index.html`


```
Welcome to Simplilearn
```


Store the index.html in configmap


```
kubectl create cm customwebpage --from-file index.html
```


Mount the configmap as a volume 


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx-custom-webapp
  name: nginx-custom-webapp
spec:
  containers:
  - image: nginx
    name: nginx-custom-webapp
    ports:
    - containerPort: 80
    volumeMounts: 
    - name: mywebpage
      mountPath: /usr/share/nginx/html
  volumes: 
  - name: mywebpage
    configMap:
      name: customwebpage 
      items: 
      - key: index.html
        path: index.html
```


Find the IP address of the pod


```
kubectl get pods nginx-custom-webapp -o wide
```


Check the web application


```
curl 192.168.39.173
```



# Persistent Volumes


# PV


```
apiVersion: v1
kind: PersistentVolume
metadata: 
 name: pv1
spec: 
 capacity: 
   storage: 1Gi
 hostPath:
  path: /tmp/data
  type: DirectoryOrCreate
 accessModes:
 - ReadWriteOnce
```



# PVC


```
apiVersion: v1
kind: PersistentVolumeClaim
metadata: 
 name: pvc1
spec: 
 resources: 
  requests: 
   storage: 500Mi
 accessModes: 
 - ReadWriteOnce
```



# PVC mounts in Pods


```
apiVersion: v1
kind: Pod
metadata:
  name: pv-pvc
spec:
  containers:
  - image: nginx
    name: nginx-custom-webapp
    ports:
    - containerPort: 80
    volumeMounts: 
    - name: mywebpage
      mountPath: /usr/share/nginx/html
  volumes: 
  - name: mywebpage
    persistentVolumeClaim: 
     claimName: pvc1
```


On the Node where pod is scheduled, populate the file “/tmp/data/index.html” with some content


```
sudo echo "Hi There. how are you" > index.html
```


Check the app in the pod


```
curl <pod-ip-address>
```



# Ingress


# Install Ingress controller


```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml
```


Get nginx controller endpoint


```
kubectl get svc -n ingress-nginx ingress-nginx-controller


```
Output
NAME                       TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller   NodePort   10.101.148.251   <none>        80:32043/TCP,443:30676/TCP   24m
```



# Deploy BackEnd Services


## Create Deployments


```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: dep1
  name: dep1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dep1
  template:
    metadata:
      labels:
        app: dep1
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: dep2
  name: dep2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dep2
  template:
    metadata:
      labels:
        app: dep2
    spec:
      containers:
      - image: httpd
        name: httpd
        ports:
        - containerPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: dep3
  name: dep3
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dep3
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: dep3
    spec:
      containers:
      - env:
        - name: TOMCAT_PASSWORD
          value: simplilearn
        image: bitnami/tomcat
        name: tomcat
        ports:
        - containerPort: 8080
```



## Expose the deployments as services:


```
kubectl expose deployment dep1 --name cart --port 80
kubectl expose deployment dep2 --name accounts --port 80
kubectl expose deployment dep3 --name orders --port 80 --target-port 8080
```



# Create Ingress 


```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata: 
 name: ingress-demo
 annotations:
  kubernetes.io/ingress.class: "nginx"
  nginx.ingress.kubernetes.io/rewrite-target: /
spec: 
 rules:
 - http:
    paths: 
    - path: /cart
      pathType: Prefix
      backend: 
       service:
        name: cart
        port: 
          number: 80
    - path: /account
      pathType: Prefix
      backend: 
       service:
        name: accounts 
        port: 
          number: 80
    - path: /orders
      pathType: Prefix
      backend: 
       service:
        name: orders
        port: 
          number: 80
```



## Explore the created ingress


```
kubectl describe ingress ingress-demo
```



# Verify the Ingress

Browse the application on any node on the cluster


```
http://localhost:<Port>/accounts 
http://localhost:<Port>/cart 
http://localhost:<Port>/orders
```





# Monitoring and Autoscaling


# Metrics Server



1. Install metrics server

    ```
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    ```


2. Edit the deployment to accept insecure TLS connection to API server:

        ```
        kubectl edit -n kube-system deployment metrics-server
        ```



        Add below line under “Args” 


        ```
                - --kubelet-insecure-tls
        ```


3. Details output of resource utilisation of pods and nodes
    1. `kubectl top pod`
    2. `kubectl top node`


# 


# Autoscale


### Create deployment with pod resource limits


```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: autoscale-dep
  name: autoscale-dep
spec:
  replicas: 3
  selector:
    matchLabels:
      app: autoscale-dep
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: autoscale-dep
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
        resources: 
         requests: 
          cpu: 100m
         limits: 
          cpu: 100m
```



### Expose the deployment


```
kubectl expose deployment autoscale-dep --name autoscale-svc --port 80
```



### Note the service IP address


```
kubectl get svc autoscale-svc


```
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
autoscale-svc   ClusterIP   10.111.85.241   <none>        80/TCP    12m
```



### Enable Auto Scaling on the deployment


```
kubectl autoscale deployment autoscale-dep --min 3 --max 8 --cpu-percent 60
```



### List the HPA


```
kubectl get hpa
```



### Generate load

On a worker node, generate load on the deployed pods, so that they will autoscale


```
apt update
apt-get install apache2-utils
ab -n 5000000 -c 1000 http://<service IP address>/
```



### Watch the autoscaling in action

On master node:


```
watch kubectl top pods -l app=autoscale-dep
```


(press ctrl+c to exit the watch command)




# Security


# Authentication


## Create a User based on Certificates

[ generate key using 2048 algorithm]


```
openssl genrsa -out alice.key 2048
```


create cert(CSR) request and assign to user alice and out to a file


```
openssl req -new -key alice.key -subj "/CN=alice" -out alice.csr
```


send the CSR for signing encode using base64


```
cat alice.csr | base64
```


Remove all newlines from the encoded csr (generated above)

Create csr.yaml and apply it


```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata: 
 name: csr-for-alice
spec: 
 groups: 
 - "system:authenticated"
 usages: 
 - client auth
 - digital signature
 - key encipherment
 signerName: kubernetes.io/kube-apiserver-client
 request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZUQ0NBVDBDQVFBd0VERU9NQXdHQTFVRUF3d0ZZV3hwWTJVd2dnRWlNQTBHQ1NxR1NJYjNEUUVCQVFVQQpBNElCRHdBd2dnRUtBb0lCQVFDandyMmZIS2xJRm9TMzJONjMrcndnaTQ4RHhUWjRmRVZ6N0c5amY0UHJyQWxmCmRZRGNOaG5oWmN4amhFelRiMGRQNFZPckFSZll6WnNSWElndTF6YWdZMVBFRVZQRmh1QUVFYXBJYUs5ZHduMVYKR2FhalhSeUNjM1JtdjBOWkFWVzc1RENlVWJxNVBrNkJwTkZBczFlQ2pwd0E5bkkxY0YwbDE0OG8wNjJ0dmx4eApvWjhWNCtjYlkvc3lDdWF4alJhNmU5WlpwL05NRkNBcjd1QVZHNEx1WnR2b0sweXV2bVZUbit6L294SDlEeUVsCmdDNTlKS3ZWZjdFMVpmenBzdGxaSGdHbnNzYVN5UlZFdyttb2pINkIzbG0zQTFJaDZJOHVNb1NOenZNOVlHZzQKVjhJU05EK0s1QWt6dkZjMGNnM3pYMm5ldjB5U08rSHVRZVlaK3hRQkFnTUJBQUdnQURBTkJna3Foa2lHOXcwQgpBUXNGQUFPQ0FRRUFHRnJxUFZ4eDMwa1lINjRJZXVQUGVxRmZSM0NLVFhRRmwraGdWRzhpNHZrSTFtbkRhUzVCCmtMRTRZNEpoajFnL3N3aUxDQ1pYL2k0SGFyZ1k0aVZZWmFqQU5rYmdNNXZCUVZKaUZ5UlU5YlhMWmVRcWR6akwKZ0EwVTljcDU2VUNyaXNJMkVxNWlicS9MYmdrLzVDODVkblUwWWdIQXorYmlNMk92M2hTV05lbjAwVDdBYlpjeAowdzBWc1B5b2hnakJmSE5NSDl1Ti9EZEZqaldGcnFuRGFZa1A2VVpCQm8zYlpPcnR5bWxTSEFBMEhsemNtV3lxCkE3dVhraFArZXFFVnJGc2orNTF1TU1Dejk3MExYMGdoVXMzazNOaXRJaVFBQWRBMnhvaHJLbDBYMDFIeUR2V2EKRlNBQ2x0QzRWU284M3RuNWIzNWV3a3pjYUdqUEpnQ3BSZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=

```


Approve the CSR


```
kubectl certificate approve csr-for-alice
```


Check the generated certificate


```
kubectl get csr csr-for-alice -o jsonpath='{.status.certificate}'
```


Save the certificate in a file


```
kubectl get csr csr-for-alice -o jsonpath='{.status.certificate}' | base64 --decode > alice.crt
```



## Use the new user in kubernetes context

Set user in kube config


```
kubectl config set-credentials alice --client-certificate alice.crt --client-key alice.key
```


Create a new context to use alice user

`kubectl config set-context alice@kubernetes --user alice --cluster kubernetes` 

Set the new context as default


```
kubectl config use-context alice@kubernetes 
```


Check the default context


```
kubectl config get-contexts
```



## Test the new user


```
kubectl get pods
```


output:


```
Error from server (Forbidden): pods is forbidden: User "alice" cannot list resource "pods" in API group "" in the namespace "default"
```



# Authorization


## Roles


### Create developer role


```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
  namespace: kube-system
rules:
- apiGroups: [""]
  resources: ['pods']
  verbs: ['get','list','create']
- apiGroups: [""]
  resources: ['ConfigMap']
  verbs: ['get','list','create','delete']
```



### Rolebinding: Developer role to Alice user


```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: alice-developer-binding
  namespace: kube-system
subjects:
- kind: User
  name: alice # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```



### Test the access



* `kubectl auth can-i delete pods`
* `kubectl auth can-i delete pods --as alice`
* `kubectl auth can-i create pods --as alice`
* `kubectl auth can-i get pods --as alice`
* `kubectl auth can-i delete namespaces --as alice`


### Execute commands as Alice user

kubectl config use-context alice@kubernetes 

kubectl get pods


## ClusterRoles


### Create Cluster role and role binding


```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["nodes"]
  verbs: ["get", "list", "create"]
- apiGroups: [""] # "" indicates the core API group
  resources: ["namespaces"]
  verbs: ["get", "list", "create"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: alice-cluster-dev-rolebinding
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-developer
  apiGroup: rbac.authorization.k8s.io

```



### Test the access


```
kubectl config use-context alice@kubernetes 
kubectl get ns
```



# NetworkPolicies


```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
 name: mynetworkpolicy
spec: 
 podSelector: 
  matchLabels: 
   app: test-dep 
 policyTypes:
 - Ingress
 ingress: 
 - from: 
   - podSelector:
      matchLabels: 
        app: simplilearn
   ports:
   - port: 80
```



# Private Registry

Create secret to store credentials to private registry


```
kubectl create secret docker-registry registry-credentials --docker-server private-registry.com --docker-username simplilearn --docker-password mysecretpassword
```


Use the credentials in a pod


```
apiVersion: v1                  
kind: Pod                       
metadata:                       
  name: private-registry-pod
spec:                            
  containers:
  - name: nginxcontainer
    image: private-registry.com/nginx
  imagePullSecrets:
  - name: registry-credentials
```



# Static Pods

On a worker Node:



1. Append the below text at the end of the last line in the file “`/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`”:
    1.  `--pod-manifest-path=/etc/kubelet.d/`
2. Make dir `/etc/kubelet.d/`
3. Reload the configuration file` `
    2. `sudo systemctl daemon-reload`
4. Restart kubelet service
    3. `sudo service kubelet restart`
5. Dump a pod manifest file in the above directory
6. Check the pods by running kubectl commands on master node

    ```
apiVersion: v1
kind: Pod
metadata:
  name: static-pod
spec: 
 containers: 
  - image: nginx
    name: nginx
```




# 


# InitContainers


```
apiVersion: v1
kind: Pod
metadata:
  name: pod-init
  labels:
    app: pod-init
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "sleep 25"]
```



# Manage Node Workloads


# Cordon



1. Cordon off worker2 node
    1. `kubectl cordon &lt;nodename>`
2. Observe the pods
    2. `kubectl get pods -o wide`
3. Check node status
    3. `kubectl get nodes`
4. Scale up a deployment
    4. `kubectl scale deployment &lt;deployment name> --replicas 10`
5. Observe the pods
    5. `kubectl get pods -o wide`


# Drain



1. Drain worker2 Node
    1. `kubectl drain worker2 --force --ignore-daemonsets `
2. Observe the pods
    2. `kubectl get pods -o wide`
3. Check node status
    3. `kubectl get nodes`
4. Uncordon the node `: `
    4. `kubectl uncordon worker2`


# Custom Scheduler

Take out the origin scheduler


```
mv /etc/kubernetes/manifests/kube-scheduler.yaml /etc/kubernetes/
```


Create file with below content (to deploy new scheduler):


```
/etc/kubernetes/manifests/my-kube-scheduler.yaml



```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    component: my-kube-scheduler
    tier: control-plane
  name: my-kube-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
    - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
    - --bind-address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=true
    - --scheduler-name=my-kube-scheduler
    - --port=0
    image: k8s.gcr.io/kube-scheduler:v1.20.11
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10259
        scheme: HTTPS
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    name: kube-scheduler
    resources:
      requests:
        cpu: 100m
    startupProbe:
      failureThreshold: 24
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10259
        scheme: HTTPS
```


Deploy an application that uses the new scheduler


```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  schedulerName: my-kube-scheduler
```




Additional Topics (if time permits):



1. k8s dashboard
2. Helm charts &lt;- difficult as time may not be enough

<!-----
NEW: Check the "Suppress top comment" option to remove this info from the output.

Conversion time: 4.16 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β31
* Sun Oct 24 2021 09:22:14 GMT-0700 (PDT)
* Source doc: Copy of AKS
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!


WARNING:
You have 3 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 16.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>
<a href="#gdcalert10">alert10</a>
<a href="#gdcalert11">alert11</a>
<a href="#gdcalert12">alert12</a>
<a href="#gdcalert13">alert13</a>
<a href="#gdcalert14">alert14</a>
<a href="#gdcalert15">alert15</a>
<a href="#gdcalert16">alert16</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


**Create a Kubernetes Cluster Using AKS**

 

** **

**Steps to be followed:**



1. Setting up the prerequisites for configuring an AKS cluster
2. Creating a Kubernetes cluster using AKS service

     


**Step** **1:** **Setting up the prerequisites for configuring an AKS cluster**


        1.1  Navigate to the Azure Portal home screen and click on the **Subscriptions **tab


        

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")



        1.2  On Subscriptions page, click on **Vocareum-SL-20 **under **Subscription name**


        

<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")



        1.3  Inside the Vocareum-SL-20 subscription, click on the **Resource groups** under **Settings**


        

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")



        1.4  On the Resource groups page, click on the resource group name to navigate inside the resource group


```
Note: Notice that the resource group name will be different for everyone, but the Subscription name will be same i.e. Vocareum-SL-20.
```



        

<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")



        1.5  Inside the resource group, click on the **Create **button and select **Marketplace**


        

<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")



        1.6  In the search box type **log analytics workspace **and select the **Log Analytics Workspace** resource from the dropdown


        

<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")



        1.7  On Log Analytics Workspace page, click on the **Create **button to create this resource


        

<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")



        1.8  On the Create Log Analytics workspace page, enter the following details and click on the **Review + Create** button:


        

<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")



    **Name:** **ContainerLogAnalytics**


    **Region: West US**


```
Note: Keep the default value for other fields.
```



        1.9  Once the validation is complete, click on the **Create **button


        

<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")



    1.10 Check the newly created resource on the **Resource group** page


    

<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")


**Step 2: Creating a Kubernetes cluster using AKS service**


        2.1  On Create a resource page, search for **Kubernetes Service** and select the **Kubernetes Service **resource from the dropdown


        

<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")



        2.2  On Kubernetes Service page, click on the **Create **button to create this resource


        2.3  On Create Kubernetes Service page, enter the following details under the **Basics **tab and click on the **Integrations** tab:


    **Kubernetes cluster name:** **SL-Container**


```
Note: Keep the default value for all the other fields. Also, make sure the Region is set as West US as all the resources should be in the same region.
```



    ** **


```
Note: AKS does not support Kubernetes 1.20 version yet.
```


** **

<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")



        2.4  On the Integrations tab, select the **Enabled **option for** Continuous monitoring **and make sure the **Log Analytics workspace** is using **ContainerLogAnalytics**. Click on the **Review + Create** button


        

<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")



        2.5  Once the validation is complete, click on the **Create **button


        

<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image14.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image14.png "image_tooltip")



        2.6  Check the resource on the **Resource group** page and click on the **SL-Cluster** resource


        

<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image15.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image15.png "image_tooltip")



        2.7  Click on Node Pool from left-side panel and check the Node pool and Nodes tab to verify the nodes in the cluster


        

<p id="gdcalert16" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image16.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert17">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image16.png "image_tooltip")



# 


# Lab exercises


## Create namespace:


```
kind: Namespace
apiVersion: v1
metadata:
  name: myns

```



## Create pod:


```
kind: Pod
apiVersion: v1
metadata:
  name: first-pod
spec:
  containers:
    - name: simplilearn
      image: nginx
```



## Create Deployment


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: first-deployment
  namespace: myns
  labels:
    app: first-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: first-deployment
  template:
    metadata:
      labels:
        app: first-deployment
    spec:
      containers:
      - name: first-deployment
        image: nginx
        ports:
        - containerPort: 80
```



## Expose Deployment


```
kubectl expose deployment -n myns first-deployment --name first-lb-svc --port 80 --type LoadBalancer
```


Identify the load Balancer IP address of the service


```
kubectl get svc -n  myns first-lb-svc



```
NAME           TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
first-lb-svc   LoadBalancer   10.0.31.236   52.161.21.175   80:32518/TCP   2m48s
```


Browse the application on the load balancer IP Address on the browser of your laptop/machine


# Dynamic Storage


```
# create PVC
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: default

---
# create a pod and mount PVC
apiVersion: v1
kind: Pod
metadata:
  name: pv-pvc
spec:
  containers:
  - image: nginx
    name: nginx-custom-webapp
    ports:
    - containerPort: 80
    volumeMounts:
    - name: mywebpage
      mountPath: /usr/share/nginx/html
  volumes:
  - name: mywebpage
    persistentVolumeClaim:
     claimName: myclaim
```

