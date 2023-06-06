# K8S - AKS


## Crear cluster AKS 

https://learn.microsoft.com/es-es/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli


### Aceder al cluster

Instalar Azure CLI
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli

Comprueba la versión instalada con: az version

```
C:\Users\alvarez\uhu\CC\k8s-aks>az version
{
  "azure-cli": "2.49.0",
  "azure-cli-core": "2.49.0",
  "azure-cli-telemetry": "1.0.8",
  "extensions": {}
}
```

Identifícate para usar el cluster con el comando: az login

```
C:\Users\alvarez\uhu\CC\k8s-aks>az login
A web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
[
  {
    "cloudName": "AzureCloud",
    ...
}
```

Define la subscripción del Clúster:

```
C:\Users\alvarez\uhu\CC\k8s-aks>az account set --subscription XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
```

Descarga las credenciales, para crear o actualizar el fichero .kube/config y establecer el nuevo clúster como el contexto por defecto:

```
C:\Users\alvarez\uhu\CC\k8s-aks>az aks get-credentials --resource-group aks-resources --name aks-cluster
Merged "aks-cluster" as current context in C:\Users\alvarez\.kube\config
```

Instala kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

```
C:\Users\alvarez\uhu\CC\k8s-aks>curl.exe -LO "https://dl.k8s.io/release/v1.25.6/bin/windows/amd64/kubectl.exe"
```

Mueve el fichero a una carpeta en el PATH y comprueba la versión del cliente

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl version --client=true -o=yaml
clientVersion:
  buildDate: "2023-01-18T19:22:09Z"
  compiler: gc
  gitCommit: ff2c119726cc1f8926fb0585c74b25921e866a28
  gitTreeState: clean
  gitVersion: v1.25.6
  goVersion: go1.19.5
  major: "1"
  minor: "25"
  platform: windows/amd64
kustomizeVersion: v4.5.7
```

O ambas, cliente y servidor (si ya te has autenticado y registrado el clúster)

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl version -o=yaml               
clientVersion:
  buildDate: "2023-01-18T19:22:09Z"
  compiler: gc
  gitCommit: ff2c119726cc1f8926fb0585c74b25921e866a28
  gitTreeState: clean
  gitVersion: v1.25.6
  goVersion: go1.19.5
  major: "1"
  minor: "25"
  platform: windows/amd64
kustomizeVersion: v4.5.7
serverVersion:
  buildDate: "2023-04-08T13:29:19Z"
  compiler: gc
  gitCommit: dc2f9dc64421983f0f7839da8ab4ab6d4673daad
  gitTreeState: clean
  gitVersion: v1.25.6
  goVersion: go1.19.5
  major: "1"
  minor: "25"
  platform: linux/amd64
```

Una lista de los principales usos de kubectl y otro con todos los comandos
```
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl/
```


Podemos obtener información del clúster con:

```
C:\Users\alvarez\OneDrive - UNIVERSIDAD DE HUELVA\CC\k8s-aks>kubectl cluster-info                            
Kubernetes control plane is running at https://aks-cluster-dns-xxxxxxxx.hcp.eastus.azmk8s.io:443
CoreDNS is running at https://aks-cluster-dns-xxxxxxxx.hcp.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://aks-cluster-dns-xxxxxxxx.hcp.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
Para la configuración del fichero .kube/config podemos usar:
```
kubectl config view

kubectl config get-contexts                          # display list of contexts
kubectl config current-context                       # display the current-context
kubectl config use-context my-cluster-name           # set the default context to my-cluster-name
```

## Consultas al clúster


APi soportadas:
```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl api-versions
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2
autoscaling/v2beta2
batch/v1
certificates.k8s.io/v1
coordination.k8s.io/v1
discovery.k8s.io/v1
events.k8s.io/v1
flowcontrol.apiserver.k8s.io/v1beta1
flowcontrol.apiserver.k8s.io/v1beta2
metrics.k8s.io/v1beta1
networking.k8s.io/v1
node.k8s.io/v1
policy/v1
rbac.authorization.k8s.io/v1
scheduling.k8s.io/v1
snapshot.storage.k8s.io/v1
snapshot.storage.k8s.io/v1beta1
storage.k8s.io/v1
storage.k8s.io/v1beta1
v1
```
Se pueden ver los posibles recursos que podemos gestionar en el clúster:

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl api-resources
NAME                              SHORTNAMES          APIVERSION                             NAMESPACED   KIND
bindings                                              v1                                     true         Binding
componentstatuses                 cs                  v1                                     false        ComponentStatus        
configmaps                        cm                  v1                                     true         ConfigMap
endpoints                         ep                  v1                                     true         Endpoints
events                            ev                  v1                                     true         Event
limitranges                       limits              v1                                     true         LimitRange
namespaces                        ns                  v1                                     false        Namespace
nodes                             no                  v1                                     false        Node
persistentvolumeclaims            pvc                 v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv                  v1                                     false        PersistentVolume       
pods                              po                  v1                                     true         Pod
podtemplates                                          v1                                     true         PodTemplate
replicationcontrollers            rc                  v1                                     true         ReplicationController  
resourcequotas                    quota               v1                                     true         ResourceQuota
secrets                                               v1                                     true         Secret
serviceaccounts                   sa                  v1                                     true         ServiceAccount
services                          svc                 v1                                     true         Service
mutatingwebhookconfigurations                         admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
validatingwebhookconfigurations                       admissionregistration.k8s.io/v1        false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds            apiextensions.k8s.io/v1                false        CustomResourceDefinition
apiservices                                           apiregistration.k8s.io/v1              false        APIService
controllerrevisions                                   apps/v1                                true         ControllerRevision     
daemonsets                        ds                  apps/v1                                true         DaemonSet
deployments                       deploy              apps/v1                                true         Deployment
replicasets                       rs                  apps/v1                                true         ReplicaSet
statefulsets                      sts                 apps/v1                                true         StatefulSet
tokenreviews                                          authentication.k8s.io/v1               false        TokenReview
localsubjectaccessreviews                             authorization.k8s.io/v1                true         LocalSubjectAccessReview
selfsubjectaccessreviews                              authorization.k8s.io/v1                false        SelfSubjectAccessReview
selfsubjectrulesreviews                               authorization.k8s.io/v1                false        SelfSubjectRulesReview 
subjectaccessreviews                                  authorization.k8s.io/v1                false        SubjectAccessReview    
horizontalpodautoscalers          hpa                 autoscaling/v2                         true         HorizontalPodAutoscaler
cronjobs                          cj                  batch/v1                               true         CronJob
jobs                                                  batch/v1                               true         Job
certificatesigningrequests        csr                 certificates.k8s.io/v1                 false        CertificateSigningRequest
leases                                                coordination.k8s.io/v1                 true         Lease
endpointslices                                        discovery.k8s.io/v1                    true         EndpointSlice
events                            ev                  events.k8s.io/v1                       true         Event
flowschemas                                           flowcontrol.apiserver.k8s.io/v1beta2   false        FlowSchema
prioritylevelconfigurations                           flowcontrol.apiserver.k8s.io/v1beta2   false        PriorityLevelConfiguration
nodes                                                 metrics.k8s.io/v1beta1                 false        NodeMetrics
pods                                                  metrics.k8s.io/v1beta1                 true         PodMetrics
ingressclasses                                        networking.k8s.io/v1                   false        IngressClass
ingresses                         ing                 networking.k8s.io/v1                   true         Ingress
networkpolicies                   netpol              networking.k8s.io/v1                   true         NetworkPolicy
runtimeclasses                                        node.k8s.io/v1                         false        RuntimeClass
poddisruptionbudgets              pdb                 policy/v1                              true         PodDisruptionBudget    
clusterrolebindings                                   rbac.authorization.k8s.io/v1           false        ClusterRoleBinding     
clusterroles                                          rbac.authorization.k8s.io/v1           false        ClusterRole
rolebindings                                          rbac.authorization.k8s.io/v1           true         RoleBinding
roles                                                 rbac.authorization.k8s.io/v1           true         Role
priorityclasses                   pc                  scheduling.k8s.io/v1                   false        PriorityClass
volumesnapshotclasses             vsclass,vsclasses   snapshot.storage.k8s.io/v1             false        VolumeSnapshotClass    
volumesnapshotcontents            vsc,vscs            snapshot.storage.k8s.io/v1             false        VolumeSnapshotContent  
volumesnapshots                   vs                  snapshot.storage.k8s.io/v1             true         VolumeSnapshot
csidrivers                                            storage.k8s.io/v1                      false        CSIDriver
csinodes                                              storage.k8s.io/v1                      false        CSINode
csistoragecapacities                                  storage.k8s.io/v1                      true         CSIStorageCapacity     
storageclasses                    sc                  storage.k8s.io/v1                      false        StorageClass
volumeattachments                                     storage.k8s.io/v1                      false        VolumeAttachment
```

Podemos solicitar información sobre un determinado recurso o de alguna de sus opciones:

```
$ kubectl explain --api-version=apps/v1 Deployment

$ kubectl explain --api-version=apps/v1 Deployment.metadata | more
$ kubectl explain --api-version=apps/v1 Deployment.spec.template | more
$ kubectl explain --api-version=apps/v1 Deployment.spec.template.spec.containers | more
```

Podemos consutar los recursos del Clúster con: kubectl get

Por ejemplo, los espacios de nombre (namespace o ns) con:

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl get ns 
NAME              STATUS   AGE
default           Active   40m
kube-node-lease   Active   40m
kube-public       Active   40m
kube-system       Active   40m
``` 

Los pods del espacio de nombre default:

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl get pods
No resources found in default namespace.
```
Podemos especifiar que espacio de nombre con: --namespace <espacio_nombres>

```
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl get pods --namespace=kube-system 
NAME                                  READY   STATUS    RESTARTS   AGE
azure-ip-masq-agent-s2lrs             1/1     Running   0          4h44m
azure-ip-masq-agent-v7jf4             1/1     Running   0          4h44m
cloud-node-manager-dg6kj              1/1     Running   0          4h44m
cloud-node-manager-hzt6t              1/1     Running   0          4h44m
coredns-6749dfb98f-642sd              1/1     Running   0          4h44m
coredns-6749dfb98f-fhbdt              1/1     Running   0          4h43m
coredns-autoscaler-6f8964bbf7-lwvl8   1/1     Running   0          4h44m
csi-azuredisk-node-shljc              3/3     Running   0          4h44m
csi-azuredisk-node-wg4sv              3/3     Running   0          4h44m
csi-azurefile-node-nvdqr              3/3     Running   0          4h44m
csi-azurefile-node-sgshd              3/3     Running   0          4h44m
konnectivity-agent-b647f99bf-k25xq    1/1     Running   0          4h18m
konnectivity-agent-b647f99bf-nglvm    1/1     Running   0          4h18m
kube-proxy-gdxwv                      1/1     Running   0          4h44m
kube-proxy-pzszd                      1/1     Running   0          4h44m
metrics-server-565c76bfcf-7wk47       2/2     Running   0          4h43m
metrics-server-565c76bfcf-slt29       2/2     Running   0          4h43m
```

```
kubectl run nginx --image=nginx --port=80 --dry-run=client -o yaml > nginx-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

Crear el Pod:
```
kubectl run nginx --image=nginx --port=80
```
o bien, si tenemos el fichero yaml:
```
kubectl create -f nginx-pod.yaml
```
ver Pod
```
kubectl get pods -o=wide
```

Ver logs
```
kubectl logs <nombre_pod>
kubectl logs nginx
```
Ejecutar comando
```
kubectl exec <nombre_pod> -- <comando>  
kubectl exec -it nginx -- bash
```

Obtener descripción de un recurso
```
kubectl describe pod nginx
```

Borrar recursos
```
kubectl delete pod <nombre_pod> 
kubectl delete pod nginx
```


Exponer el Pod a internet mediante un servicio

```
kubectl expose pod nginx --port=80 --type=LoadBalancer --name=frontend
service/frontend exposed
```

Borrar Pod y Servicio:
```
kubectl delete all -l run=nginx 

C:\Users\alvarez\uhu\CC\k8s-aks>kubectl delete service frontend
service "frontend" deleted
C:\Users\alvarez\uhu\CC\k8s-aks>kubectl delete pod nginx                                              
pod "nginx" deleted
```

## Crear una aplicación y un deployment en Kubernetes

Aplicación Go

```
package main

import (
        "fmt"
        "log"
        "net/http"
        "os"
)

func handler(w http.ResponseWriter, r *http.Request) {

        hostname, err := os.Hostname()
        if err != nil {
                fmt.Fprintln(w, "Error:", err)
        } else {
                fmt.Fprintln(w, "Hello from", hostname, "(version 1)")
        }
}

func main() {
        http.HandleFunc("/", handler)
        log.Println("Go Hello is listening on port 8888")
        log.Fatal(http.ListenAndServe(":8888", nil))
}
```
Fichero Dockerfile

```
FROM golang:1.20.4-alpine AS build
WORKDIR /src/
COPY main.go /src/
RUN go mod init example/hello
RUN CGO_ENABLED=0 go build -o /bin/demo

FROM scratch
COPY --from=build /bin/demo /bin/demo
ENTRYPOINT ["/bin/demo"]
```
Crea la imagen:

```
$ docker build -t jluisalvarez/go_hello:2023 .

$ docker login

$ docker push jluisalvarez/go_hello:2023
```

Crear el deployment

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deployment
  labels:
    app: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: go_hello
        image: jluisalvarez/go_hello:v1
        ports:
        - containerPort: 8888
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
```

```
$ kubectl apply -f hello_deploy.yaml
```

## Exponer el deployment en un servicio


```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: hello
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8888
  type: LoadBalancer
```

```
$ kubectl apply -f hello_service.yaml
```


## Escalar la aplicación


A patir del fichero
```
kubectl scale --replicas=5 -f hello_deploy.yaml
```
El recurso concreto
```
kubectl scale --replicas=3 deployment/hello-deployment
```

Autoescalado
```
kubectl autoscale deployment hello-deployment --max 6 --min 3 --cpu-percent 50


for ((i=1;i<=10000000;i++)); do   curl --header "Connection: keep-alive" "http://104.45.168.138:8080/"; done
```

Ejemplo:
```
https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
```


## Nueva versión

kubectl set image deployments/hello-deployment go_hello=jluisalvarez/go_hello:v2

Ver versiones
```
kubectl rollout history deployment/hello-deployment
```
Anotaciones del motivo del cambio
```
kubectl annotate deployment/hello-deployment kubernetes.io/change-cause="New version"
``` 
Comprobar estado de la actualización:
```
kubectl rollout status deployments/hello-deployment
```

Deshacer actualización:
```
kubectl rollout undo deployments/hello-deployment
```





## Más ejemplos

Para continuar puedes usar este repo:
https://github.com/HoussemDellai/ProductsStoreOnKubernetes









