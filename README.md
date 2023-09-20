## 1. **Storage Classes**

**Descripción:** Las Storage Classes representan diferentes niveles de almacenamiento en la infraestructura subyacente.

**Ejemplo:**

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-sc
provisioner: ebs.csi.aws.com
volumeBindingMode: WaitForFirstConsumer
```

En este ejemplo usamos EBS CSI Controller:

https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html

---

## 2. **Volumenes en Kubernetes**

**Descripción:** Los volúmenes proporcionan almacenamiento a los contenedores dentro de un Pod.

**Ejemplo:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sample-pod
  namespace: game-2048
spec:
  containers:
  - image: nginx
    name: nginx-container
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: game-volume
  volumes:
  - name: game-volume
    emptyDir: {}
```

---

## 3. **Persistent Volume (PV)**

**Descripción:** Representa un pedazo de almacenamiento en el cluster, independiente de su provisión.

**Ejemplo:**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ebs-pv
  namespace: game-2048
spec:
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: ebs-sc
```

---

## 4. **Persistent Volume Claims (PVC)**

**Descripción:** Es una solicitud de almacenamiento por un usuario, que puede ser vinculada a un PV.

**Ejemplo:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-claim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: ebs-sc
  resources:
    requests:
      storage: 4Gi
```

---

## 5. **Montar Volumenes en un Pod**

**Descripción:** Asociar un volumen a un contenedor en un Pod, asegurando que los datos estén disponibles.

**Ejemplo:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: game-webserver
  namespace: game-2048
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: game-storage
  volumes:
  - name: game-storage
    persistentVolumeClaim:
      claimName: game-pvc
```
