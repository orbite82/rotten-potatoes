# AULA 3

```
cp ~/Downloads/iniciativa-k8s-kubeconfig.yaml ~/.kube/config 
```
```
kubectl get nodes
NAME                 STATUS   ROLES    AGE   VERSION
pool-default-usm28   Ready    <none>   10m   v1.21.5
pool-default-usm2c   Ready    <none>   10m   v1.21.5
pool-default-usm2u   Ready    <none>   10m   v1.21.5
```
```
cd rotten-potatoes/
cd k8s/
```
```
kubectl apply -f deployment.yaml 
deployment.apps/mongodb created
service/mongodb created
deployment.apps/web created
service/web created
```
```
kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mongodb-9f45bf784-sjf8l   1/1     Running   0          42s
pod/web-f966df955-nwbz5       1/1     Running   0          42s

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.245.0.1       <none>        443/TCP        22m
service/mongodb      ClusterIP   10.245.158.75    <none>        27017/TCP      43s
service/web          NodePort    10.245.245.184   <none>        80:30000/TCP   42s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb   1/1     1            1           43s
deployment.apps/web       1/1     1            1           43s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-9f45bf784   1         1         1       43s
replicaset.apps/web-f966df955       1         1         1       43s
```
# Alterado pra loadbalancer arquivo deployment

```
kubectl apply -f deployment.yaml 
deployment.apps/mongodb unchanged
service/mongodb unchanged
deployment.apps/web unchanged
service/web configured
```
```
kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mongodb-9f45bf784-sjf8l   1/1     Running   0          3m56s
pod/web-f966df955-nwbz5       1/1     Running   0          3m56s

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP      10.245.0.1       <none>        443/TCP        26m
service/mongodb      ClusterIP      10.245.158.75    <none>        27017/TCP      3m57s
service/web          LoadBalancer   10.245.245.184   <pending>     80:30000/TCP   3m56s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb   1/1     1            1           3m57s
deployment.apps/web       1/1     1            1           3m57s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-9f45bf784   1         1         1       3m57s
replicaset.apps/web-f966df955       1         1         1       3m57s
```
```
watch 'kubectl get all'
```
esperando gerar o ip externo

```
kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mongodb-9f45bf784-sjf8l   1/1     Running   0          11m
pod/web-f966df955-nwbz5       1/1     Running   0          11m

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGE
service/kubernetes   ClusterIP      10.245.0.1       <none>           443/TCP        33m
service/mongodb      ClusterIP      10.245.158.75    <none>           27017/TCP      11m
service/web          LoadBalancer   10.245.245.184   167.172.13.216   80:30000/TCP   11m

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb   1/1     1            1           11m
deployment.apps/web       1/1     1            1           11m

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-9f45bf784   1         1         1       11m
replicaset.apps/web-f966df955       1         1         1       11m
```

# Teste no seu navegador

```
167.172.13.216
```
# Removendo o deployment

```
kubectl delete -f deployment.yaml 
deployment.apps "mongodb" deleted
service "mongodb" deleted
deployment.apps "web" deleted
service "web" deleted
```
# Pipeline

https://github.com/orbite82/rotten-potatoes/actions/new
Skip this and set up a workflow yourself -->

altera e comita no github actions
faça o teste de hello world
editar -> ... view workflow file do lado esquerdo
edit no lapis
em search marketplace for actions -> usar Docker Login = https://github.com/marketplace/actions/docker-login
e Build and push Docker images = https://github.com/marketplace/actions/build-and-push-docker-images

usar -> Kubernetes Set Context para meu CD
usar -> Deploy to Kubernetes cluster

# Deploy efetuado via github actions

```
 kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mongodb-9f45bf784-fw55m   1/1     Running   0          39s
pod/web-cc669555f-5nr7w       1/1     Running   0          39s

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.245.0.1       <none>        443/TCP        126m
service/mongodb      ClusterIP   10.245.184.245   <none>        27017/TCP      39s
service/web          NodePort    10.245.104.240   <none>        80:30000/TCP   39s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb   1/1     1            1           39s
deployment.apps/web       1/1     1            1           39s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-9f45bf784   1         1         1       39s
replicaset.apps/web-cc669555f       1         1         1       39s
```

# Alterando o service para deploy com LoadBalancer

```
kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mongodb-9f45bf784-fw55m   1/1     Running   0          8m34s
pod/web-755469ff76-l7w6b      1/1     Running   0          3m45s

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)        AGE
service/kubernetes   ClusterIP      10.245.0.1       <none>            443/TCP        134m
service/mongodb      ClusterIP      10.245.184.245   <none>            27017/TCP      8m35s
service/web          LoadBalancer   10.245.104.240   206.189.252.106   80:30000/TCP   8m35s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb   1/1     1            1           8m35s
deployment.apps/web       1/1     1            1           8m35s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-9f45bf784   1         1         1       8m35s
replicaset.apps/web-755469ff76      1         1         1       3m46s
replicaset.apps/web-cc669555f       0         0         0       8m35s
```

# Agora posso deletar meu deployment e subir pelo github actions que vai funcionar normal o deploy

```
kubectl delete -f deployment.yaml
```

```
kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
mongodb-9f45bf784-fw55m   1/1     Running   0          22m
web-755469ff76-l7w6b      1/1     Running   0          17m
```

```
kubectl describe pod web-755469ff76-l7w6b
Name:         web-755469ff76-l7w6b
Namespace:    default
Priority:     0
Node:         pool-default-usm2u/10.116.0.4
Start Time:   Fri, 21 Jan 2022 12:37:27 -0300
Labels:       app=web
              pod-template-hash=755469ff76
Annotations:  githubWorkflow_b8eebe382f5d856c0c04fa4893da162c:
                {"run":"1729469484","repository":"orbite82/rotten-potatoes","workflow":"CI-CD","workflowFileName":"main.yml","jobName":"CD","createdBy":"o...
Status:       Running
IP:           10.244.0.41
IPs:
  IP:           10.244.0.41
Controlled By:  ReplicaSet/web-755469ff76
Containers:
  web:
    Container ID:   containerd://784e5e7727e32198c552853cab92e60c35ad062eff63aec2bac5432c19862ebc
    Image:          orbite82/rotten-potatoes:14
    Image ID:       docker.io/orbite82/rotten-potatoes@sha256:e7ca7926dd2bef7dec2b2b3691c7f560b6142380048f5033f68870f5017add6a
    Port:           5000/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 21 Jan 2022 12:37:34 -0300
    Ready:          True
    Restart Count:  0
    Environment:
      MONGODB_DB:        admin
      MONGODB_HOST:      mongodb
      MONGODB_PORT:      27017
      MONGODB_USERNAME:  mongouser
      MONGODB_PASSWORD:  mongopwd
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-qw8k4 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-qw8k4:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/web-755469ff76-l7w6b to pool-default-usm2u
  Normal  Pulling    18m   kubelet            Pulling image "orbite82/rotten-potatoes:14"
  Normal  Pulled     18m   kubelet            Successfully pulled image "orbite82/rotten-potatoes:14" in 3.949252928s
  Normal  Created    17m   kubelet            Created container web
  Normal  Started    17m   kubelet            Started container web
```  
