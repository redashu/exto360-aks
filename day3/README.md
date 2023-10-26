# exto360-aks

### checking cluster details 

```
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get no
NAME                                STATUS   ROLES   AGE   VERSION
aks-agentpool-39082632-vmss000004   Ready    agent   12m   v1.26.6
aks-agentpool-39082632-vmss000005   Ready    agent   11m   v1.26.6
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  ns
NAME              STATUS   AGE
amit-projec       Active   16h
anand-project     Active   16h
arvind-project    Active   16h
ashu-project      Active   16h
calico-system     Active   41h
default           Active   41h
ko-project        Active   16h
kube-node-lease   Active   41h
kube-public       Active   41h
kube-system       Active   41h
megha-project     Active   16h
praveen-peoject   Active   16h
rahul-project     Active   16h
shal              Active   16h
shilpa-project    Active   16h
siva              Active   16h
tigera-operator   Active   41h
```

### checking current namespace

```
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl config get-contexts 
CURRENT   NAME              CLUSTER           AUTHINFO                                          NAMESPACE
*         ext-360-cluster   ext-360-cluster   clusterUser_container-expertise_ext-360-cluster   ashu-project
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
```

### checking config view 

```
kubectl  config  view 
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://ext-360-cluster-dns-ztmjkln9.hcp.centralindia.azmk8s.io:443
  name: ext-360-cluster
contexts:
- context:
    cluster: ext-360-cluster
    namespace: ashu-project
    user: clusterUser_container-expertise_ext-360-cluster
  name: ext-360-cluster
current-context: ext-360-cluster
kind: Config
preferences: {}
users:
- name: clusterUser_container-expertise_ext-360-cluster
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
    token: REDACTED
```

### pods can communicate to each other 

```
 kubectl  run -it --rm  --image=alpine  --command sh 


If you don't see a command prompt, try pressing enter.

/ # 
/ # 
/ # ping  10.244.0.5
PING 10.244.0.5 (10.244.0.5): 56 data bytes
64 bytes from 10.244.0.5: seq=0 ttl=63 time=0.214 ms
64 bytes from 10.244.0.5: seq=1 ttl=63 time=0.090 ms
64 bytes from 10.244.0.5: seq=2 ttl=63 time=0.112 ms
^C
```

### Creating deployment 

```
[ashu@ip-172-31-60-143 ashu-apps]$ ls
apache-server  ashu-python  java-code  k8s-res-design  node-app  webui-app
[ashu@ip-172-31-60-143 ashu-apps]$ cd  k8s-res-design/
[ashu@ip-172-31-60-143 k8s-res-design]$ ls
ashupod1.yaml  ashupod2.yaml  autopod.yaml  deploy1.yaml  sts.yaml
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl   create -f deploy1.yaml 
deployment.apps/ashu-deploy1 created
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl   get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   0/1     1            0           4s
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  po 
NAME                            READY   STATUS              RESTARTS   AGE
ashu-deploy1-5ccff9465f-s8s6z   0/1     ContainerCreating   0          8s
pod1                            1/1     Running             0          8m29s
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  delete pod pod1 
pod "pod1" deleted
[ashu@ip-172-31-60-143 k8s-res-design]$ 
```

### scaling pod to 2 

<img src="net1.png">

### scaling 

```
ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  no
NAME                                STATUS   ROLES   AGE   VERSION
aks-agentpool-39082632-vmss000004   Ready    agent   77m   v1.26.6
aks-agentpool-39082632-vmss000005   Ready    agent   77m   v1.26.6
aks-agentpool-39082632-vmss000006   Ready    agent   41m   v1.26.6
[ashu@ip-172-31-60-143 k8s-res-design]$ 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   1/1     1            1           29m
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  scale  deployment ashu-deploy1  --replicas 2
deployment.apps/ashu-deploy1 scaled
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   1/2     2            1           30m
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl get po -o wide
NAME                            READY   STATUS              RESTARTS   AGE   IP            NODE                                NOMINATED NODE   READINESS GATES
ashu-deploy1-5ccff9465f-lw7c5   0/1     ContainerCreating   0          11s   <none>        aks-agentpool-39082632-vmss000006   <none>           <none>
ashu-deploy1-5ccff9465f-s8s6z   1/1     Running             0          30m   10.244.0.10   aks-agentpool-39082632-vmss000004   <none>           <none>
[ashu@ip-172-31-60-143 k8s-res-design]$ 
```

### service discovery in k8s 

<img src="svc1.png">

### introduction to service resource under apiversion v1 

```
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  create service
Create a service using a specified subcommand.

Aliases:
service, svc

Available Commands:
  clusterip      Create a ClusterIP service
  externalname   Create an ExternalName service
  loadbalancer   Create a LoadBalancer service
  nodeport       Create a NodePort service
```

### creating internal LB using clusterip type service

```
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  create service   clusterip  ashu-web-lb  --tcp  1234:80  --dry-run=client -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashu-web-lb
  name: ashu-web-lb
spec:
  ports:
  - name: 1234-80
    port: 1234
    protocol: TCP
    targetPort: 80
  selector:
    app: ashu-web-lb
  type: ClusterIP
status:
  loadBalancer: {}
```

### creating it 

```
kubectl  create service   clusterip  ashu-web-lb  --tcp  1234:80  --dry-run=client -o yaml >web_lb.yaml
[ashu@ip-172-31-60-143 k8s-res-design]$ 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  create -f web_lb.yaml 
service/ashu-web-lb created
[ashu@ip-172-31-60-143 k8s-res-design]$ 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get service 
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
ashu-web-lb   ClusterIP   10.0.240.221   <none>        1234/TCP   5s
[ashu@ip-172-31-60-143 k8s-res-design]$ 

```

### easitest method

```
kubectl   create  -f deploy1.yaml 
deployment.apps/ashu-deploy1 created
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   1/1     1            1           7s
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  po 
NAME                            READY   STATUS    RESTARTS   AGE
ashu-deploy1-5ccff9465f-std9d   1/1     Running   0          26s
```

### using expose to create service 

```
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   1/1     1            1           2m46s
[ashu@ip-172-31-60-143 k8s-res-design]$ 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  expose  deployment  ashu-deploy1  --type ClusterIP --port 80 --name ashu-web-lb  --dry-run=client  -o yaml  >auto_web_lb.yaml 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  create -f auto_web_lb.yaml 
service/ashu-web-lb created
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  service
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
ashu-web-lb   ClusterIP   10.0.242.126   <none>        80/TCP    6s
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  ep 
NAME          ENDPOINTS        AGE
ashu-web-lb   10.244.2.32:80   12s
[ashu@ip-172-31-60-143 k8s-res-design]$ 
```
### Creating internal and external LB using Loadbalancer service type

```
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  svc
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
ashu-web-lb   ClusterIP   10.0.242.126   <none>        80/TCP    19m
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl   delete svc  ashu-web-lb 
service "ashu-web-lb" deleted
[ashu@ip-172-31-60-143 k8s-res-design]$ 


```

### creating lb
```
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
ashu-deploy1   1/1     1            1           25m
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  expose deployment ashu-deploy1 --type LoadBalancer --port 80 --name ashu-lb-inext --dry-run=client  -o yaml >int_ext_lb.yaml 
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  create -f int_ext_lb.yaml 
service/ashu-lb-inext created
[ashu@ip-172-31-60-143 k8s-res-design]$ kubectl  get  svc
NAME            TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
ashu-lb-inext   LoadBalancer   10.0.190.49   <pending>     80:30785/TCP   4s
[ashu@ip-172-31-60-143 k8s-res-design]$

```

### service in overall way 

<img src="svc11.png">


## deploying node and mongo app 

```
 mkdir my-node-mongo-app
[ashu@ip-172-31-60-143 ashu-apps]$ cd my-node-mongo-app/
[ashu@ip-172-31-60-143 my-node-mongo-app]$ ls
[ashu@ip-172-31-60-143 my-node-mongo-app]$ 

```

### Creating node-js-app as deployment controller 

```
kubectl   create  deploy ashu-node  --image=extoaksashu.azurecr.io/node-app:v1   --port 3000 --dry-run=client -o yaml  >node_deploy.yaml 
```

### having yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-node
  name: ashu-node # name of deployment 
spec:
  replicas: 1 # number of pod we want 
  selector:
    matchLabels:
      app: ashu-node
  strategy: {}
  template: # to create pods 
    metadata:
      creationTimestamp: null
      labels: # label of all the pods 
        app: ashu-node
    spec:
      containers:
      - image: extoaksashu.azurecr.io/node-app:v1 # from ACR 
        name: node-app
        ports:
        - containerPort: 3000
        resources: {}
status: {}

```




