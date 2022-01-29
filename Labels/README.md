# Labels, Selectors y Annotations

Labels: Sirven para etiquetar objetos. A travez de ellas se pueden realizar relaciones. No admiten espacios y no usar las etiquetas del sistema.Si puede contener puntos.

Selectors: identificar objewtos en las busqeudas y las relaciones

Annotations: describir o comentar un objeto para que le quede claro a otro desarrollador

```
$ vi tomcat.yaml
apiVercion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
spec:
  containers:
   - name: tomcat
     image: tomcat
```

```
$ kubectl apply -f tomcat.yaml
$ kubectl get pods
$ kubectl get pods -o wide
$ kubectl get pod tomcat --show-labels
```
Con la opcion -L me agrega una columnas con los nombres indicados. Si no hay una etiqeuta declarada con ese nombre, como por ejemplo apli me la deja vacia, u si hay una etiquetado me completa la columna con su valor.

```
$ kubectl get pod tomcat --show-labels -L estado
$ kubectl get pod tomcat --show-labels -L estado,apli
NAME    READY   STATUS    RESTARTS    AGE   ESTADO     API   LABELS
tomcat  1/1     Running   0           5m45s desarrollo        estado=desarrollo        
```

## Modificar etiquetas

modo imperativo:

Agrego una etiqueta

```
$ kubectl label pod tomcat responsable=juan
$ kubectl get pods --show-labels
$ kubectl get pods --show-labels -L estado,responsable
NAME    READY   STATUS    RESTARTS    AGE   ESTADO     RESPONSABLE LABELS
tomcat  1/1     Running   0           5m45s desarrollo juan        estado=desarrollo,reponsable=juan
```
Modificar una etiqueta

```
$ kubectl label --overwrite pod/tomcat estado=test
$ **$ kubectl get pods --show-labels -L estado,responsable
NAME    READY   STATUS    RESTARTS    AGE   ESTADO     RESPONSABLE LABELS
tomcat  1/1     Running   0           5m45s test       juan        estado=test,reponsable=juan
```
Eliminar una etiqueta

```
$ kubectl label pod/tomcat responsable-
$ **$ kubectl get pods --show-labels -L estado,responsable
NAME    READY   STATUS    RESTARTS    AGE   ESTADO     RESPONSABLE LABELS
tomcat  1/1     Running   0           5m45s test                   estado=test,reponsable=juan
```

modo imperativo:

Modifico el archivo tomcat-yaml

```
$ vi tomcat.yaml
apiVercion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
    responsable: "rosa"
spec:
  containers:
   - name: tomcat
     image: tomcat
$ kubectl apply -f tomcat.yaml
$ **$ kubectl get pods --show-labels -L estado,responsable
NAME    READY   STATUS    RESTARTS    AGE   ESTADO     RESPONSABLE LABELS
tomcat  1/1     Running   0           5m45s desarrollo rosa                   estado=test,reponsable=juan
```

 Si quiero quitar las etiquetas, las borro del archivo tomcat-yaml y vuelvo hacer un apply (lo mismo si las quiero modificar).

```
$ kubectl describe pod/tomcat
```
 
 
 



