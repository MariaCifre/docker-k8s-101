# Step 06 - Actualizar la aplicacion (rolling update)

## Objetivo del step

Replicar "Performing a Rolling Update" con cambio de imagen via manifiesto y `apply`.

## Fundamento del step

Actualizar por manifiestos garantiza trazabilidad del cambio y facilita rollback.

## Ejecucion guiada

### 1) Cambiar imagen del Deployment

En `labs/03-k8s-basico/trabajo/k8s/manifests/01-deployment.yaml`, cambia:

```yaml
image: gcr.io/google-samples/kubernetes-bootcamp:v1
```

por:

```yaml
image: gcr.io/google-samples/kubernetes-bootcamp:v2
```

### 2) Aplicar cambio

```bash
kubectl apply -f labs/03-k8s-basico/trabajo/k8s/manifests/01-deployment.yaml
```

### 3) Seguir rollout

```bash
kubectl -n k8s-basics rollout status deployment/kubernetes-bootcamp
kubectl -n k8s-basics get pods -l app=kubernetes-bootcamp
```

### 4) Verificar version en uso

```bash
kubectl -n k8s-basics describe deployment kubernetes-bootcamp | rg "Image:"
```

## Que validas y que debes ver

- Rollout completado sin downtime perceptible.
- Pods nuevos con la imagen `v2`.

## Errores comunes

- Tag de imagen inexistente.
- No esperar a `rollout status` antes de validar.

## Reto

Haz rollback a la version anterior.

## Solucion del reto

```bash
kubectl -n k8s-basics rollout undo deployment/kubernetes-bootcamp
kubectl -n k8s-basics rollout status deployment/kubernetes-bootcamp
```

## Navegacion del libro

- [Anterior](05-escalar-la-aplicacion.md)
- [Siguiente](../../04-k8s-analitica/README.md)
