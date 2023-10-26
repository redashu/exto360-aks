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

