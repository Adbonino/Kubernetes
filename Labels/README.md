# Labels

Sirven para etiquetar objetos. A travez de ellas se pueden realizar relaciones.

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
