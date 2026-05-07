# Step 04 - Exponer la app con Service (manifiestos)

## Objetivo del step

Replicar "Using a Service to Expose Your App" usando YAML en lugar de `kubectl expose`.

## Fundamento del step

El Service da un punto de entrada estable aunque los pods cambien.

## Ejecucion guiada

### 1) Crear manifiesto de Service

Archivo: `labs/03-k8s-basico/trabajo/k8s/manifests/02-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: kubernetes-bootcamp
  namespace: k8s-basics
  labels:
    app: kubernetes-bootcamp
spec:
  type: NodePort
  selector:
    app: kubernetes-bootcamp
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080
```

### 2) Aplicar y describir

```bash
kubectl apply -f labs/03-k8s-basico/trabajo/k8s/manifests/02-service.yaml
kubectl -n k8s-basics get services
kubectl -n k8s-basics describe services kubernetes-bootcamp
```
NAME                  TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes-bootcamp   NodePort   10.96.149.96   <none>        8080:30080/TCP   6s

IP:                       10.96.149.96
IPs:                      10.96.149.96
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30080/TCP
Endpoints:                10.244.0.5:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Internal Traffic Policy:  Cluster
Events:                   <none>

### 3) Ver labels y selectors en accion

```bash
kubectl -n k8s-basics get pods -l app=kubernetes-bootcamp
kubectl -n k8s-basics get services -l app=kubernetes-bootcamp
```
NAME                                  READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-978dd9cbc-gsxbl   1/1     Running   0          20m

NAME                  TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes-bootcamp   NodePort   10.96.149.96   <none>        8080:30080/TCP   80s
## Que validas y que debes ver

- Service tipo `NodePort` creado.
- Endpoints asociados a pods del deployment.

## Errores comunes

- `selector` no coincide con `app: kubernetes-bootcamp`.
- NodePort bloqueado en el entorno.

## Reto

Probar acceso a la app por `NodePort`.

## Solucion del reto

```bash
curl -sS http://localhost:30080/
```
curl: (7) Failed to connect to localhost port 30080 after 0 ms: Couldn't connect to server

Si no abre NodePort en tu entorno:

```bash
kubectl -n k8s-basics port-forward service/kubernetes-bootcamp 8080:8080
curl -sS http://127.0.0.1:8080/
```
@MariaCifre ➜ /workspaces/docker-k8s-101 (main) $ curl -sS http://127.0.0.1:8080/
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-978dd9cbc-gsxbl | v=1

## Navegacion del libro

- [Anterior](03-ver-pods-y-nodos.md)
- [Siguiente](05-escalar-la-aplicacion.md)
