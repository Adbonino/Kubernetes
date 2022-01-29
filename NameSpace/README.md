# NameSpaces (Agrupar nuestros objetos en Kubernetes)

Particiones para poner mis objetos.(Nombre logico para agrupar objetos).
El NameSpaces es una división lógica del cluster para dividir el cluster en zonas.

kube-system: namespace del sistema

$ kubectl get namespace
$ kubectl get namespace default
$ kubectl get namespace kube-system
$ kubectl describe namespace default
$ kubectl get pods -n kube-system

Crear y borrar namespaces.
forma imperativa
$ kubectl create namespace n1
$ kubectl get namespace
$ kubectl describe namespace n1
$ kubectl get pods -n n1

forma declarativa
$ kubectl apply -f namespace.yaml
$ kubectl get namespace
$ kubectl describe namespace dev1

$ kubectl delete namespace n1 

$ vi deploy_elastic.yaml 
apiVersion: apps/v1 # i se Usa apps/v1beta2 para versiones anteriores a 1.9.0
kind: Deployment
metadata:
  name: elastic
  labels:
    tipo: "desarrollo"
spec:
  selector:   #permite seleccionar un conjunto de objetos que cumplan las condicione
    matchLabels:
      app: elastic
  replicas: 2 # indica al controlador que ejecute 2 pods
  strategy:
     type: RollingUpdate
  minReadySeconds: 2
  template:   # Plantilla que define los containers
    metadata:
      labels:
        app: elastic
    spec:
      containers:
      - name: elastic
        image: elasticsearch:7.6.0    #agrego 7.6.0 para qeu ande.
        ports:
        - containerPort: 9200
$ kubectl apply -f deploy_elastic --namespace=dev1
$ kubectl get deploy elastic -n dev1
$ kubectl get pod -n dev1
$ kubectl describe deploy elastic -n dev1
$ kubectl get rs -n dev1
$ kubectl get pods -n dev1
$ kubectl describe pod elactic-...... -n dev1
da un error not found vengo a docker hub (le puse latest y seguramoente no hay lastest por defecto.)
$ kubectl apply -f deploy_elastic.yaml -n dev1 
$ kubectl get pods -n dev1
$ kubectl rollout history deploy elastic -n dev1

COmohacer que nuetrso namespace sea enl de defecto.(para evita rponer todo el tiempo -n dev1)
$ ls -la .kube
$ cd .kube
$ cat config #moidifico el archivo de confgi y arranco de nuevo el cluster

Como lo hago en caliente
$ kubectl config set-context--current --namespace=dev1
$ kubectl config view   #aca puedo ver que se m odifico el namespace por defecto.

Poner CPU y memoria a los namespace.

$ kubectl create namspacer n1
$ vi limites.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: recursos
spec:
  limits:
  - default:
      memory: 512Mi
      cpu: 1
    defaultRequest:
      memory: 256Mi
      cpu: 0.5
    max:
      memory: 1Gi
      cpu: 4
    min:
      memory: 128Mi
      cpu: 0.5
    type: Container

$ kubectl apply -f limites.yaml -n n1   # si no le pongo -n n1 los limites los aplica al namespace por default
$ kubectl describe namespace n1
$ kubectl run nginx --image=nginx -n n1 
