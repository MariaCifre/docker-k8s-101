# Step 05 - Escalar la aplicacion

## Objetivo del step

Replicar "Running Multiple Instances of Your App" de forma declarativa.

## Fundamento del step

Escalar replicas en el manifiesto mantiene el estado deseado documentado en Git.

## Ejecucion guiada

### 1) Editar replicas en el Deployment

En `labs/03-k8s-basico/trabajo/k8s/manifests/01-deployment.yaml`, cambia:

```yaml
replicas: 4
```

### 2) Reaplicar manifiesto

```bash
kubectl apply -f labs/03-k8s-basico/trabajo/k8s/manifests/01-deployment.yaml
```
deployment.apps/kubernetes-bootcamp configured

### 3) Verificar escalado

```bash
kubectl -n k8s-basics get deployments
kubectl -n k8s-basics get pods -l app=kubernetes-bootcamp
```
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   4/4     4            4           42m

NAME                                  READY   STATUS    RESTARTS      AGE
kubernetes-bootcamp-978dd9cbc-dhmts   1/1     Running   0             21s
kubernetes-bootcamp-978dd9cbc-gsxbl   1/1     Running   1 (15m ago)   42m
kubernetes-bootcamp-978dd9cbc-trfdl   1/1     Running   0             21s
kubernetes-bootcamp-978dd9cbc-wmq8z   1/1     Running   0             21s

## Que validas y que debes ver

- `READY` del deployment en `4/4`.
- Cuatro pods `Running`.

## Errores comunes

- Editar otro archivo distinto al manifiesto aplicado.
- Reaplicar namespace en vez de deployment.

## Reto

Baja a 2 replicas y confirma que Kubernetes elimina pods sobrantes.

## Solucion del reto

```bash
kubectl apply -f labs/03-k8s-basico/trabajo/k8s/manifests/01-deployment.yaml
kubectl -n k8s-basics get pods -l app=kubernetes-bootcamp
```
NAME                                  READY   STATUS        RESTARTS      AGE
kubernetes-bootcamp-978dd9cbc-dhmts   1/1     Terminating   0             56s
kubernetes-bootcamp-978dd9cbc-gsxbl   1/1     Running       1 (15m ago)   43m
kubernetes-bootcamp-978dd9cbc-trfdl   1/1     Terminating   0             56s
kubernetes-bootcamp-978dd9cbc-wmq8z   1/1     Running       0             56s

## Navegacion del libro

- [Anterior](04-exponer-app-con-service.md)
- [Siguiente](06-actualizar-la-aplicacion.md)
