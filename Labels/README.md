# Labels, Selectors y Annotations

Labels: Sirven para etiquetar objetos. A travez de ellas se pueden realizar relaciones.
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
