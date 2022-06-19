# Kubernetes Lab4

1- How many ConfigMaps exist in the environment?
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get cm --all-namespaces
NAMESPACE         NAME                                 DATA   AGE
default           kube-root-ca.crt                     1      3d
finance           kube-root-ca.crt                     1      36h
kube-node-lease   kube-root-ca.crt                     1      3d
kube-public       cluster-info                         1      3d
kube-public       kube-root-ca.crt                     1      3d
kube-system       coredns                              1      3d
kube-system       extension-apiserver-authentication   6      3d
kube-system       kube-proxy                           2      3d
kube-system       kube-root-ca.crt                     1      3d
kube-system       kubeadm-config                       1      3d
kube-system       kubelet-config-1.23                  1      3d
```

2- Create a new ConfigMap Use the spec given below.
ConfigName Name: webapp-config-map
Data: APP_COLOR=darkblue

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl create cm webapp-config-map \
--from-literal=APP_COLOR=darkblue
configmap/webapp-config-map created
```
3- Create a webapp-color POD with nginx image and use the created
ConfigMap

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp-color
  labels:
    app: nginx-webserver


spec:
  containers:
  - name: nginx
    image: nginx:alpine
    envFrom:
    - configMapRef:
        name: webapp-config-map
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl create -f web-app-color.yml
pod/webapp-color created
```
4- How many Secrets exist on the system?

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get secrets --all-namespaces
NAMESPACE         NAME                                             TYPE                                  DATA   AGE
default           default-token-dj69p                              kubernetes.io/service-account-token   3      3d
finance           default-token-gdwxq                              kubernetes.io/service-account-token   3      37h
kube-node-lease   default-token-jkmh9                              kubernetes.io/service-account-token   3      3d
kube-public       default-token-8kvrh                              kubernetes.io/service-account-token   3      3d
kube-system       attachdetach-controller-token-lkvq5              kubernetes.io/service-account-token   3      3d
kube-system       bootstrap-signer-token-gdhzj                     kubernetes.io/service-account-token   3      3d
kube-system       certificate-controller-token-s2sfq               kubernetes.io/service-account-token   3      3d
kube-system       clusterrole-aggregation-controller-token-kvkk8   kubernetes.io/service-account-token   3      3d
kube-system       coredns-token-pq7n6                              kubernetes.io/service-account-token   3      3d
kube-system       cronjob-controller-token-qxmwd                   kubernetes.io/service-account-token   3      3d
kube-system       daemon-set-controller-token-js8gj                kubernetes.io/service-account-token   3      3d
kube-system       default-token-mx6zw                              kubernetes.io/service-account-token   3      3d
kube-system       deployment-controller-token-cqbwz                kubernetes.io/service-account-token   3      3d
kube-system       disruption-controller-token-tw4qz                kubernetes.io/service-account-token   3      3d
kube-system       endpoint-controller-token-xzc97                  kubernetes.io/service-account-token   3      3d
kube-system       endpointslice-controller-token-ht7xf             kubernetes.io/service-account-token   3      3d
kube-system       endpointslicemirroring-controller-token-5vzk9    kubernetes.io/service-account-token   3      3d
kube-system       ephemeral-volume-controller-token-bk5k6          kubernetes.io/service-account-token   3      3d
kube-system       expand-controller-token-qvrmh                    kubernetes.io/service-account-token   3      3d
kube-system       generic-garbage-collector-token-sjr5x            kubernetes.io/service-account-token   3      3d
kube-system       horizontal-pod-autoscaler-token-9jvt4            kubernetes.io/service-account-token   3      3d
kube-system       job-controller-token-kcl97                       kubernetes.io/service-account-token   3      3d
kube-system       kube-proxy-token-nt46j                           kubernetes.io/service-account-token   3      3d
kube-system       namespace-controller-token-v5bj7                 kubernetes.io/service-account-token   3      3d
kube-system       node-controller-token-hghkn                      kubernetes.io/service-account-token   3      3d
kube-system       persistent-volume-binder-token-h9h87             kubernetes.io/service-account-token   3      3d
kube-system       pod-garbage-collector-token-4npjb                kubernetes.io/service-account-token   3      3d
kube-system       pv-protection-controller-token-85mzz             kubernetes.io/service-account-token   3      3d
kube-system       pvc-protection-controller-token-gd2gz            kubernetes.io/service-account-token   3      3d
kube-system       replicaset-controller-token-qbrbc                kubernetes.io/service-account-token   3      3d
kube-system       replication-controller-token-8dqrs               kubernetes.io/service-account-token   3      3d
kube-system       resourcequota-controller-token-5252b             kubernetes.io/service-account-token   3      3d
kube-system       root-ca-cert-publisher-token-w8zd2               kubernetes.io/service-account-token   3      3d
kube-system       service-account-controller-token-br7m6           kubernetes.io/service-account-token   3      3d
kube-system       service-controller-token-scgsh                   kubernetes.io/service-account-token   3      3d
kube-system       statefulset-controller-token-s9zkf               kubernetes.io/service-account-token   3      3d
kube-system       storage-provisioner-token-9klqc                  kubernetes.io/service-account-token   3      3d
kube-system       token-cleaner-token-ctmw2                        kubernetes.io/service-account-token   3      3d
kube-system       ttl-after-finished-controller-token-knp5d        kubernetes.io/service-account-token   3      3d
kube-system       ttl-controller-token-m2mnz                       kubernetes.io/service-account-token   3      3d
```

5- How many secrets are defined in the default-token secret?

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-dj69p   kubernetes.io/service-account-token   3      3d
```
6- create a POD called db-pod with the image mysql:5.7 then check the
POD status.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
  labels:
    app: db
    tier: backend

spec:
  containers:
  - name: mysql-container
    image: mysql:5.7
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl create -f db-pod.yml 
pod/db-pod created

amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS    RESTARTS      AGE
pod/db-pod         0/1     Error     2 (24s ago)   108s
pod/webapp-color   1/1     Running   0             11m
#Pod status (ERROR) because the mysql pod needs an ENV VARIABLE to be set first
```
7- why the db-pod status not ready

Pod status (ERROR) because the mysql pod needs an ENV VARIABLE to be set first

8- Create a new secret named db-secret with the data given below.
Secret Name: db-secret
​ Secret 1: MYSQL_DATABASE=sql01
​ Secret 2: MYSQL_USER=user1
​ Secret3: MYSQL_PASSWORD=password
​ Secret 4: MYSQL_ROOT_PASSWORD=password123

```yaml
#db-pod-secret.yml file
apiVersion: v1
kind: Secret
metadata:
  creationTimestamp: null
  name: db-secret
data:
  MYSQL_DATABASE: c3FsMDE=
  MYSQL_PASSWORD: cGFzc3dvcmQ=
  MYSQL_ROOT_PASSWORD: cGFzc3dvcmQxMjM=
  MYSQL_USER: dXNlcjE=
```
```yaml
#db-pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
  labels:
    app: db
    tier: backend

spec:
  containers:
  - name: mysql-container
    image: mysql:5.7
    envFrom:
    - secretRef:
        name: db-secret
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS    RESTARTS   AGE
pod/db-pod         1/1     Running   0          2m22s
pod/webapp-color   1/1     Running   0          22m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22m
```
9- Configure db-pod to load environment variables from the newly created
secret.
Delete and recreate the pod if required.

Done
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS    RESTARTS   AGE
pod/db-pod         1/1     Running   0          2m22s
pod/webapp-color   1/1     Running   0          22m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22m
```

10- Create a multi-container pod with 2 containers.
Name: yellow
Container 1 Name: lemon
Container 1 Image: busybox
Container 2 Name: gold
Container 2 Image: redis

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: yellow
  labels:
    app: db
    tier: backend

spec:
  containers:
  - name: lemon
    image:  busybox
    command: ["sleep"]
    args: ["300"]
  - name: gold
    image: redis
```

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS    RESTARTS   AGE
pod/db-pod         1/1     Running   0          15m
pod/webapp-color   1/1     Running   0          35m
pod/yellow         2/2     Running   0          7s
```
11- Create a pod red with redis image and use an initContainer that
uses the busybox image and sleeps for 20 seconds

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: red
  labels:
    app: redis

spec:
  containers:
  - name: redis
    image: redis
  
  initContainers:
  - name: init-container
    image: busybox:latest
    command: ["sleep"]
    args: ["20"]
```

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS     RESTARTS   AGE
pod/db-pod         1/1     Running    0          19m
pod/red            0/1     Init:0/1   0          22s
pod/webapp-color   1/1     Running    0          40m
pod/yellow         2/2     Running    0          4m42s
```

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get all
NAME               READY   STATUS    RESTARTS   AGE
pod/db-pod         1/1     Running   0          19m
pod/red            1/1     Running   0          33s
pod/webapp-color   1/1     Running   0          40m
pod/yellow         2/2     Running   0          4m53s

```
12- Create a pod named print-envars-greeting.
1. Configure spec as, the container name should be
print-env-container and use bash image.
2. Create three environment variables:
 a. GREETING and its value should be “Welcome to”
 b. COMPANY and its value should be “DevOps”
 c. GROUP and its value should be “Industries”
4. Use command to echo ["$(GREETING) $(COMPANY) $(GROUP)"]
message.
5. You can check the output using <kubctl logs -f [ pod-name ]>
command.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting

spec:
  containers:
  - name: print-envars-greeting-container
    image: bash
    command: ["echo"]
    args: ["$(GREETING) $(COMPANY) $(GROUP)"]



    env:
    - name: GREETING
      value: "Welcome to"

    - name: COMPANY
      value: "DevOps"

    - name: GROUP
      value: "Industries"
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl logs -f pod/print-envars-greeting
Welcome to DevOps Industries
```


13- Where is the default kubeconfig file located in the current environment?

/home/amir/.minikube/

14- How many clusters are defined in the default kubeconfig file?

```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl config get-clusters
NAME
minikube
```

15- What is the user configured in the current context?
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl config current-context
minikube
```

16- Create a Persistent Volume with the given specification.
Volume Name: pv-log
​ Storage: 100Mi
​ Access Modes: ReadWriteMany
​ Host Path: /pv/log

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log

spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/log
```

```bash
lienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get pv
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-log   100Mi      RWX            Retain           Available                                   2m7s
```
17- Create a Persistent Volume Claim with the given specification.
​ Volume Name: claim-log-1
​ Storage Request: 50Mi
​ Access Modes: ReadWriteMany

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
```
```bash
amir@amir-Alienware-x15-R1:~/KubernetesLabs/KubernetesLabsITI/Lab4$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                 STORAGECLASS   REASON   AGE
pv-log                                     100Mi      RWX            Retain           Available                                                 13m
pvc-bbdf146a-d09d-4ff3-ba34-d2e210303b63   50Mi       RWX            Delete           Bound       default/claim-log-1   standard                6m57s
```

18- Create a webapp pod to use the persistent volume claim as its storage.
Name: webapp
​ Image Name: nginx
​ Volume: PersistentVolumeClaim=claim-log-1
​ Volume Mount: /var/log/nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp

spec:
  containers:
    - name: nginx-cont
      image: nginx
      volumeMounts:
        - mountPath: /var/log/nginx
          name: myvol
  volumes:
    - name: myvol
      persistentVolumeClaim:
        claimName: claim-log-1
```