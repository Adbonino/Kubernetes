# Labels, Selectors y Annotations

Labels: Sirven para etiquetar objetos. A travez de ellas se pueden realizar relaciones. No admiten espacios y no usar las etiquetas del sistema.Si puede contener puntos.

Selectors: identificar objewtos en las busqeudas y las relaciones

Annotations: describir o comentar un objeto para que le quede claro a otro desarrollador

## Labels

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

### Modificar etiquetas

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

modo declarativco:

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
 
## Selectors

Muy importante para encontrar objetos y es el objeto primario qeu se usa para relacionar componentes.
Lo selectores con como where y los utilizo para localizar objetos qeu tengan determinadas etiquetas.

Ej: buscame todos los pods que tengan como etiqueta desarrollo.

```
$ vi tomcat.yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
    responsable: "juan"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```

```
$ vi tomcat1.yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat1
  labels:
    estado: "desarrollo"
    responsable: "juan"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```   
     
```
$ vi tomcat2.yaml   
apiVersion: v1
kind: Pod
metadata:
  name: tomcat2
  labels:
    estado: "testing"
    responsable: "pedro"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```

```
$ vi tomcat3.yaml   
apiVersion: v1
kind: Pod
metadata:
  name: tomcat3
  labels:
    estado: "produccion"
    responsable: "pedro"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```

aplico todos los ficheros qeu hay dentro del directorio

```
$ kubectl apply -f .
$ kubectl get pods
NAME      READY   STATUS    RESTARTS    AGE
tomcat    1/1     Runnine   0           14s
tomcat1   1/1     Runnine   0           14s
tomcat2   1/1     Runnine   0           14s
tomcat3   1/1     Runnine   0           14s
$ kubectl get pods --show-labels
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
tomcat2   1/1     Runnine   0           14s   estado=testing,responsable=pedro
tomcat3   1/1     Runnine   0           14s   estado=produccion,reponsable=pedro  
```

Como busco con los selectores puedo usar la opcion -l o --selectors

```
$ kubectl get pods --show-labels -l estado=desarrollo
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
$ kubectl get pods --show-labels -l estado=testing
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat2   1/1     Runnine   0           14s   estado=testing,responsable=pedro
$ kubectl get pods --show-labels -l estado==testing
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat2   1/1     Runnine   0           14s   estado=testing,responsable=pedro
```

Condicion tipo and
 
```
$ kubectl get pods --show-labels -l estado=desarrollo,responsable=juan
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
```

Negacion

```
$ kubectl get pods --show-labels -l responsable!=juan
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat2   1/1     Runnine   0           14s   estado=testing,responsable=pedro
tomcat3   1/1     Runnine   0           14s   estado=produccion,reponsable=pedro  
$ kubectl get pods --show-labels -l estado!=testing
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
tomcat3   1/1     Runnine   0           14s   estado=produccion,reponsable=pedro  
```

Trabajar mediante conjuntos

```
$ kubectl get pods --show-labels -l 'estado in(desarrollo)' 
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
$ kubectl get pods --show-labels -l 'estado in(desarrollo,testing)'
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat    1/1     Runnine   0           14s   estado=desarrollo,responsable=juan 
tomcat1   1/1     Runnine   0           14s   estado=desarrollo,responsable=juan
tomcat2   1/1     Runnine   0           14s   estado=testing,responsable=pedro
$ kubectl get pods --show-labels -l 'estado notin(desarrollo,testing)'
NAME      READY   STATUS    RESTARTS    AGE   LABELS
tomcat3   1/1     Runnine   0           14s   estado=produccion,reponsable=pedro 
```

Borra todos los pods que tengas la etiqueta estado=desarrollo

```
$ kubectl delete pods -l estado=desarrollo
```

## Anotaciones

Son descriptivas, nos permite documentacion, comentarios y demas (sirven para describir y documentar a nuestros componentes)

```
$ vi tomcat4
apiVersion: v1
kind: Pod
metadata:
  name: tomcat4
  labels:
    estado: "produccion"
    responsable: "pedro"
  annotations:
    doc: "Se debe compilar con gcc"
    adjunto: "ejemplo de anotacion"
spec:
  containers:
   - name: tomcat     
     image: tomcat
$ kubectl apply -f tomcat4
$ kubectl get pod tomcat4
$ kubectl describe pod tomcat4    # no hay un get especifico para las anotaciones
$ kubectl get pod tomcat4 -o jsonpath={.metadata.annotations}
```

