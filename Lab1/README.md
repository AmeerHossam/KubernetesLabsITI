# Kubernetes Lab1 ITI

1- Run Mini Kube

```
$ minikube start
```
2- Create a pod with the name redis and with the image redis.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-image
  labels:
    app: my-app
    type: database

spec:
  containers:
    - name : redis-container
      image : redis
```
3- Create a pod with the name nginx and with the image “nginx123”
Use a pod-definition YAML file.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-image
  labels:
    app: my-app
    type: database

spec:
  containers:
    - name : nginx
      image : nginx123

```

```bash
#The Pod won't create because the image name is wrong
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get po
NAME          READY   STATUS         RESTARTS   AGE
nginx-image   0/1     ErrImagePull   0          8s
```
4- What is the nginx pod status?
```bash
#ErrorImagePull
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get po
NAME          READY   STATUS         RESTARTS   AGE
nginx-image   0/1     ErrImagePull   0          8s
```

5- Change the nginx pod image to “nginx” check the status again
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-image
  labels:
    app: my-app
    type: database

spec:
  containers:
    - name : nginx
      image : nginx
```
```bash
#When we changed the image to nginx ,the pod runs fine
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get po
NAME          READY   STATUS    RESTARTS   AGE
nginx-image   1/1     Running   0          4s
```

6- How many ReplicaSets exist on the system?
```bash
#There is no replicas in my system
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
No resources found in default namespace.
```
7- create a ReplicaSet with
name= replica-set-1
image= busybox
replicas= 3
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replica-set-1
  labels:
    app: mybusy-box
    type: os

spec:
  replicas: 3
  template:
    metadata:
      name: mybusybox-pod
      labels:
        app: mybusybox
        type: os

    spec:
      containers:
        - name: busy-box-container
          image: busybox

  selector:
    matchLabels:
      type: os
```
```bash
#When I run kubectl create -f name of yaml file , It creates the Replicas
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
NAME            DESIRED   CURRENT   READY   AGE
replica-set-1   3         3         0       8s
```
8- Scale the ReplicaSet replica-set-1 to 5 PODs.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replica-set-1
  labels:
    app: mybusy-box
    type: os

spec:
  replicas: 5
  template:
    metadata:
      name: mybusybox-pod
      labels:
        app: mybusybox
        type: os

    spec:
      containers:
        - name: busy-box-container
          image: busybox

  selector:
    matchLabels:
      type: os
```
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl apply -f busy-box.yml 
replicaset.apps/replica-set-1 configured
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
NAME            DESIRED   CURRENT   READY   AGE
replica-set-1   5         5         0       2m20s
```
9- How many PODs are READY in the replica-set-1?
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
NAME            DESIRED   CURRENT   READY   AGE
replica-set-1   5         5         0       2m20s
```

10- Delete any one of the 5 PODs then check How many PODs exist now?
Why are there still 5 PODs, even after you deleted one?

```
There are 5 PODS still running , because when we delete a pod , the controller manager detects that there
is a pod died so it sends to the scheduler that there is a pod died and scheduler sends to API server and
the apiServer sends to the kubelet to ensure that rebuilding a new one.
```
11- How many Deployments and ReplicaSets exist on the system?
- 1 replicasets
- 0 deployments
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
NAME            DESIRED   CURRENT   READY   AGE
replica-set-1   5         5         0       2m20s
```
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get deployment
No resources found in default namespace.
```
12- create a Deployment with
name= deployment-1
image= busybox
replicas= 3
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
  labels:
    app: mybusy-box
    type: os

spec:
  replicas: 3
  template:
    metadata:
      name: mybusybox-pod
      labels:
        app: mybusybox
        type: os

    spec:
      containers:
        - name: busy-box-container
          image: busybox

  selector:
    matchLabels:
      type: os
```

13- How many Deployments and ReplicaSets exist on the system now?
- 1 Deployment
- 1 Replicasets
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get deployment
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
deployment-1   0/3     3            0           5s

amir@amir-Alienware-x15-R1:~/K8S$ kubectl get rs
NAME            DESIRED   CURRENT   READY   AGE
replica-set-1   5         5         0       2m20s
```

14- How many pods are ready with the deployment-1?

- 0 pods
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get deployment
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
deployment-1   0/3     3            0           5m28s
```

15- Update deployment-1 image to nginx then check the ready pods again

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: replica-set-1
  labels:
    app: mybusy-box
    type: os

spec:
  replicas: 5
  template:
    metadata:
      name: mybusybox-pod
      labels:
        app: mybusybox
        type: os

    spec:
      containers:
        - name: busy-box-container
          image: nginx
          command: ["sleep"]
          args: ["10"]

  selector:
    matchLabels:
      type: os
```
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get all
NAME                                 READY   STATUS              RESTARTS         AGE
pod/deployment-1-6679d8d765-ppbkc    0/1     CrashLoopBackOff    10 (3m29s ago)   30m
pod/nginx-image                      1/1     Running             0                85m
pod/replica-set-1-7bd76c7698-7hnmj   0/1     ContainerCreating   0                1s
pod/replica-set-1-7bd76c7698-9z5pn   0/1     ContainerCreating   0                1s
pod/replica-set-1-7bd76c7698-bjwp9   0/1     ContainerCreating   0                1s
pod/replica-set-1-7bd76c7698-g8pqc   0/1     ContainerCreating   0                1s
pod/replica-set-1-7bd76c7698-slznp   0/1     ContainerCreating   0                1s
pod/replica-set-1-7gqhm              0/1     CrashLoopBackOff    20 (29s ago)     79m
pod/replica-set-1-jbb4q              0/1     CrashLoopBackOff    20 (39s ago)     79m
pod/replica-set-1-s6zm6              0/1     CrashLoopBackOff    19 (4m3s ago)    79m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   33h

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment-1    0/3     3            0           40m
deployment.apps/replica-set-1   0/5     5            0           1s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-1-6679d8d765    1         1         0       30m
replicaset.apps/replica-set-1              3         3         0       81m
replicaset.apps/replica-set-1-7bd76c7698   5         5         0       1s
```

16- Run kubectl describe deployment deployment-1 and check events
What is the deployment strategy used to upgrade the deployment-1?

```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl describe deployment deployment-1 | grep Strategy
StrategyType:           RollingUpdate
```
17- Rollback the deployment-1
What is the used image with the deployment-1?

```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl rollout undo deploy deployment-1 --to-revision=1
deployment.apps/deployment-1 rolled back
```
18- Create a deployment using nginx image with latest tag only and remember to
mention tag i.e nginx:latest and name it as nginx-deployment. App labels should be
app: nginx-app and type: front-end. The container should be named as
nginx-container; also make sure replica counts are 3

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
    type: frontend

spec:
  replicas: 3
  selector:
    matchLabels:
      type: frontend

  template:
    metadata:
      name: nginx-Pod
      labels:
        app: nginx-app
        type: frontend
      

    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
```
```bash
amir@amir-Alienware-x15-R1:~/K8S$ kubectl get all
NAME                                    READY   STATUS              RESTARTS         AGE
pod/deployment-1-6679d8d765-ppbkc       0/1     CrashLoopBackOff    11 (3m27s ago)   35m
pod/nginx-deployment-6bb57469bf-7gv86   1/1     Running             0                8s
pod/nginx-deployment-6bb57469bf-k5lxh   1/1     Running             0                8s
pod/nginx-deployment-6bb57469bf-mh7rc   0/1     ContainerCreating   0                8s
pod/nginx-image                         1/1     Running             0                90m
pod/replica-set-1-7bd76c7698-7hnmj      0/1     CrashLoopBackOff    5 (87s ago)      5m14s
pod/replica-set-1-7bd76c7698-9z5pn      0/1     CrashLoopBackOff    5 (96s ago)      5m14s
pod/replica-set-1-7bd76c7698-bjwp9      0/1     CrashLoopBackOff    5 (110s ago)     5m14s
pod/replica-set-1-7bd76c7698-g8pqc      0/1     CrashLoopBackOff    5 (2m5s ago)     5m14s
pod/replica-set-1-7bd76c7698-slznp      0/1     CrashLoopBackOff    5 (85s ago)      5m14s
pod/replica-set-1-7gqhm                 0/1     CrashLoopBackOff    21 (34s ago)     84m
pod/replica-set-1-jbb4q                 0/1     CrashLoopBackOff    21 (46s ago)     84m
pod/replica-set-1-s6zm6                 0/1     CrashLoopBackOff    20 (4m9s ago)    84m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   33h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment-1       0/3     3            0           46m
deployment.apps/nginx-deployment   2/3     3            2           8s
deployment.apps/replica-set-1      0/5     5            0           5m14s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-1-6679d8d765       1         1         0       35m
replicaset.apps/nginx-deployment-6bb57469bf   3         3         2       8s
replicaset.apps/replica-set-1                 3         3         0       86m
replicaset.apps/replica-set-1-7bd76c7698      5         5         0       5m14s
```
