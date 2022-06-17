# K8S Lab2

1- How many Namespaces exist on the system?
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl get namespaces
NAME              STATUS   AGE
default           Active   34h
kube-node-lease   Active   34h
kube-public       Active   34h
kube-system       Active   34h
```
2- How many pods exist in the kube-system namespace?
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl get pods -n kube-system
NAME                               READY   STATUS    RESTARTS     AGE
coredns-64897985d-82h4w            1/1     Running   2 (8h ago)   34h
etcd-minikube                      1/1     Running   2 (8h ago)   34h
kube-apiserver-minikube            1/1     Running   2 (8h ago)   34h
kube-controller-manager-minikube   1/1     Running   2 (8h ago)   34h
kube-proxy-rjwvs                   1/1     Running   2 (8h ago)   34h
kube-scheduler-minikube            1/1     Running   2 (8h ago)   34h
storage-provisioner                1/1     Running   5 (8h ago)   34h
```
3- Create a deployment with
Name: beta
Image: redis
Replicas: 2
Namespace: finance
Resources Requests:
CPU: .5 vcpu
Mem: 1G
Resources Limits:
CPU: 1 vcpu
Mem: 2G

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: beta
  namespace: finance
  labels:
    app: redis-app
    type: DB

spec:
  replicas: 2
  selector:
    matchLabels:
      type: DB

  template:
    metadata:
      name: redis-pod
      labels:
        app: redis-app
        type: DB

    spec:
      containers:
      - name: beta
        image: redis
        resources:
          requests:
            cpu: ".5"
            memory: "1Gi"
          limits:
            cpu: "1"
            memory: "2Gi"
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl create namespace finance
namespace/finance created
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl create -f redis-deployment.yml 
deployment.apps/beta created
```
4- How many Nodes exist on the system?
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl get node
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   35h   v1.23.3
```
5- Do you see any taints on master?
```
No
```
6- Apply a label color=blue to the master node
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl label nodes minikube color=blue
node/minikube labeled
node/minikube labeled
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl get nodes --show-labels
NAME       STATUS   ROLES                  AGE   VERSION   LABELS
minikube   Ready    control-plane,master   35h   v1.23.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,color=blue,kubernetes.io/arch=amd64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux,minikube.k8s.io/
commit=362d5fdc0a3dbee389b3d3f1034e8023e72bd3a7,minikube.k8s.io/
name=minikube,minikube.k8s.io/
primary=true,minikube.k8s.io/updated_at=2022_06_16T12_45_10_0700,minikube.k8s.io/
version=v1.25.2,node-role.kubernetes.io/
control-plane=,node-role.kubernetes.io/master=,node.kubernetes.io/exclude-from-external-load-balancers=
```
7- Create a new deployment named blue with the nginx image and 3 replicas
Set Node Affinity to the deployment to place the pods on master only
NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
Key: color
values: blue

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
  labels:
    app: webServer1

spec:
  replicas: 3
  selector:
    matchLabels:
      app: webServer1
  
  template:
    metadata:
      name: nginx-pod
      labels:
        app: webServer1


    spec:
      containers:
      - name : nginx1
        image : nginx:latest
      
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab2$ kubectl get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/blue-748446744d-5j2hf   1/1     Running   0          13s
pod/blue-748446744d-nfvq7   1/1     Running   0          13s
pod/blue-748446744d-wdwkl   1/1     Running   0          13s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   35h

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/blue   3/3     3            3           13s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/blue-748446744d   3         3         3       13s
```