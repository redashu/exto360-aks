# exto360-aks

### app containerization with git and docker server

<img src="ds.png">

### k8s arch rev

<img src="rev.png">

### checking cluster info

```
kubectl  cluster-info 
Kubernetes control plane is running at https://ext-360-cluster-dns-ztmjkln9.hcp.centralindia.azmk8s.io:443
CoreDNS is running at https://ext-360-cluster-dns-ztmjkln9.hcp.centralindia.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://ext-360-cluster-dns-ztmjkln9.hcp.centralindia.azmk8s.io:443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

```

### deleteing all namespace data

```
kubectl  delete all --all
pod "ashu-mongo-7b968fd595-gp7zt" deleted
pod "ashu-node-59f86797d6-4x6ss" deleted
pod "ashu-node-59f86797d6-9496n" deleted
pod "ashu-node-59f86797d6-9882w" deleted
pod "ashu-node-59f86797d6-kkdvn" deleted
pod "ashu-node-59f86797d6-zkhcn" deleted
service "ashu-mongo-lb" deleted
service "ashu-node-lb" deleted
deployment.apps "ashu-mongo" deleted
deployment.apps "ashu-node" deleted
horizontalpodautoscaler.autoscaling "ashu-node" deleted
```

### few commands for admin

```
 kubectl  delete all --all
  584  kubectl  get secrets
  585  kubectl  get  po 
  586  kubectl  get  po  -A
  587  kubectl  get  secret  -A
  588  kubectl  get  secret  --all-namespaces 
  589  history 
  590  kubectl  get  all -n amit-projec
  591  kubectl  get  all -n amit-project
  592  kubectl  -n  amit-project  exec -it  amit-node-87886cfc5-2796b  -- bash 
```

### deleting it 

```
kubectl  apply  -f my-node-mongo-app/
secret/ashu-reg-cred configured
secret/ashu-db-cred configured
horizontalpodautoscaler.autoscaling/ashu-node configured
deployment.apps/ashu-mongo configured
service/ashu-mongo-lb configured
deployment.apps/ashu-node configured
service/ashu-node-lb configured
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl delete  -f my-node-mongo-app/
secret "ashu-reg-cred" deleted
secret "ashu-db-cred" deleted
horizontalpodautoscaler.autoscaling "ashu-node" deleted
deployment.apps "ashu-mongo" deleted
service "ashu-mongo-lb" deleted
```

### k8s yaml packaging 

<img src="helm1.png">

### helm version 

<img src="helm2.png">

### verify helm and kubectl in windows 

```
 history

  Id CommandLine
  -- -----------
   1 helm version
   2 kubectl version --client -o yaml
   3 kubectl  get nodes
   4 kubectl  config  get-contexts
   5 kubectl get  ns
   6 kubectl config set-context --current --namespace default


PS C:\Users\humanfirmware> kubectl  config  get-contexts
CURRENT   NAME              CLUSTER           AUTHINFO                                          NAMESPACE
*         ext-360-cluster   ext-360-cluster   clusterUser_container-expertise_ext-360-cluster   default
PS C:\Users\humanfirmware>
PS C:\Users\humanfirmware>
PS C:\Users\humanfirmware>
PS C:\Users\humanfirmware>
PS C:\Users\humanfirmware>
```

### checking repo list

```

[ashu@ip-172-31-60-143 ashu-apps]$ helm repo ls
Error: no repositories to show
[ashu@ip-172-31-60-143 ashu-apps]$ 

```

### adding repo in helm 

```
 helm  repo  add  ashu-repo  https://charts.bitnami.com/bitnami
"ashu-repo" has been added to your repositories
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm repo ls
NAME            URL                               
ashu-repo       https://charts.bitnami.com/bitnami
[ashu@ip-172-31-60-143 ashu-apps]$ 
```

### adding more repo 

```
helm repo ls
NAME            URL                                               
ashu-repo       https://charts.bitnami.com/bitnami                
ashu-monitor    https://prometheus-community.github.io/helm-charts
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm repo add  ashu-personal https://redashu.github.io/test-helm/
"ashu-personal" has been added to your repositories
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm repo ls
NAME            URL                                               
ashu-repo       https://charts.bitnami.com/bitnami                
ashu-monitor    https://prometheus-community.github.io/helm-charts
ashu-personal   https://redashu.github.io/test-helm/            
```

### deploy first helm chart package 

```
helm repo ls
NAME            URL                                               
ashu-repo       https://charts.bitnami.com/bitnami                
ashu-monitor    https://prometheus-community.github.io/helm-charts
ashu-personal   https://redashu.github.io/test-helm/              
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm search  repo  nginx
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                                       
ashu-monitor/prometheus-nginx-exporter  0.2.0           0.11.0          A Helm chart for the Prometheus NGINX Exporter    
ashu-personal/nginx                     0.1.0           1.16.0          A Helm chart for Kubernetes                       
ashu-repo/nginx                         15.3.5          1.25.3          NGINX Open Source is a web server that can be a...
ashu-repo/nginx-ingress-controller      9.9.2           1.9.3           NGINX Ingress Controller is an Ingress controll...
ashu-repo/nginx-intel                   2.1.15          0.4.9           DEPRECATED NGINX Open Source for Intel is a lig...
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get all
No resources found in ashu-project namespace.
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm install  ashu-app  ashu-repo/nginx  
NAME: ashu-app
LAST DEPLOYED: Fri Oct 27 06:59:58 2023
NAMESPACE: ashu-project
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 15.3.5
APP VERSION: 1.25.3

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    ashu-app-nginx.ashu-project.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:
```

### to see currently deployed charts

```
helm  ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
ashu-app        ashu-project    1               2023-10-27 06:59:58.1340088 +0000 UTC   deployed        nginx-15.3.5    1.25.3     
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
```

### helm verify the deployment thing 

```
kubectl  get deploy
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app-nginx   1/1     1            1           3m48s
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl   get po 
NAME                              READY   STATUS    RESTARTS   AGE
ashu-app-nginx-75c6d758df-6xp5l   1/1     Running   0          4m16s
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl   get  svc
NAME             TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
ashu-app-nginx   LoadBalancer   10.0.119.62   20.219.172.89   80:31370/TCP   4m21s
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  ep
NAME             ENDPOINTS          AGE
ashu-app-nginx   10.244.2.59:8080   5m3s
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  secret
NAME                             TYPE                 DATA   AGE
sh.helm.release.v1.ashu-app.v1   helm.sh/release.v1   1      5m19s
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  hpa
No resources found in ashu-project namespace.
```

### remove / uninstalling helm deployment 

```
helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
ashu-app        ashu-project    1               2023-10-27 06:59:58.1340088 +0000 UTC   deployed        nginx-15.3.5    1.25.3     
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm uninstall ashu-app  
release "ashu-app" uninstalled
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ helm ls
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ 
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  all
NAME                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
service/ashu-app-nginx   LoadBalancer   10.0.119.62   20.219.172.89   80:31370/TCP   7m29s
[ashu@ip-172-31-60-143 ashu-apps]$ kubectl  get  all
No resources found in ashu-project namespace.
[ashu@ip-172-31-60-143 ashu-apps]$ 
```

### creating helm chart to understand its internal structure 

```
ls
apache-server  ashu-python  java-code  k8s-res-design  my-node-mongo-app  node-app  webui-app
[ashu@ip-172-31-60-143 ashu-apps]$ ls
apache-server  ashu-python  helm-explore  java-code  k8s-res-design  my-node-mongo-app  node-app  webui-app
[ashu@ip-172-31-60-143 ashu-apps]$ cd helm-explore/
[ashu@ip-172-31-60-143 helm-explore]$ ls
[ashu@ip-172-31-60-143 helm-explore]$ helm create  ashu-webapp 
Creating ashu-webapp
[ashu@ip-172-31-60-143 helm-explore]$ ls
ashu-webapp
[ashu@ip-172-31-60-143 helm-explore]$ ls  ashu-webapp/
charts  Chart.yaml  templates  values.yaml
[ashu@ip-172-31-60-143 helm-explore]$ 

```

### main focus is values.yaml

```
 cd ashu-webapp/
[ashu@ip-172-31-60-143 ashu-webapp]$ ls
charts  Chart.yaml  templates  values.yaml
[ashu@ip-172-31-60-143 ashu-webapp]$ ls templates/
deployment.yaml  _helpers.tpl  hpa.yaml  ingress.yaml  NOTES.txt  serviceaccount.yaml  service.yaml  tests
[ashu@ip-172-31-60-143 ashu-webapp]$ 


```

### deploy it with custom values.yaml

### values.yaml 
```
service:
  type: ClusterIP
```
### ---

```
cd  modify-helm-charts/
[ashu@ip-172-31-60-143 modify-helm-charts]$ ls
values.yaml
[ashu@ip-172-31-60-143 modify-helm-charts]$ 
[ashu@ip-172-31-60-143 modify-helm-charts]$ helm install  ashu-app  ashu-repo/nginx   --values values.yaml 
NAME: ashu-app
LAST DEPLOYED: Fri Oct 27 07:33:08 2023
NAMESPACE: ashu-project
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 15.3.5
APP VERSION: 1.25.3

```
### verify 

```
 helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
ashu-app        ashu-project    1               2023-10-27 07:33:08.530844335 +0000 UTC deployed        nginx-15.3.5    1.25.3     
[ashu@ip-172-31-60-143 modify-helm-charts]$ 
[ashu@ip-172-31-60-143 modify-helm-charts]$ kubectl  get deploy
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app-nginx   1/1     1            1           77s
[ashu@ip-172-31-60-143 modify-helm-charts]$ kubectl  get svc
NAME             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
ashu-app-nginx   ClusterIP   10.0.30.117   <none>        80/TCP    82s
```

