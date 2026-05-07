# Step 03 - Ver Pods y Nodos (troubleshooting)

## Objetivo del step

Seguir el flujo "Viewing Pods and Nodes" para inspeccionar lo desplegado.

## Fundamento del step

Antes de exponer la app, necesitas dominar `get`, `describe`, `logs` y `exec`.

## Ejecucion guiada

### 1) Ver estado general

```bash
kubectl get nodes
kubectl -n k8s-basics get pods
```

### 2) Inspeccionar pod en detalle

```bash
kubectl -n k8s-basics describe pods
```

### 3) Revisar logs

```bash
kubectl -n k8s-basics logs -l app=kubernetes-bootcamp
```

### 4) Acceder al pod con exec

```bash
POD_NAME="$(kubectl -n k8s-basics get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' | head -n 1)"
kubectl -n k8s-basics exec "$POD_NAME" -- env
```

## Que validas y que debes ver

- Eventos de scheduling en `describe`.

Exemple:
Name:             kubernetes-bootcamp-978dd9cbc-gsxbl
Namespace:        k8s-basics
Priority:         0
Service Account:  default
Node:             k8s-101-control-plane/172.19.0.3
Start Time:       Thu, 07 May 2026 12:43:15 +0000
Labels:           app=kubernetes-bootcamp
                  pod-template-hash=978dd9cbc
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:           10.244.0.5
Controlled By:  ReplicaSet/kubernetes-bootcamp-978dd9cbc
Containers:
  kubernetes-bootcamp:
    Container ID:   containerd://93f40e215a03c40bab848075ae9e906eb2721fe44d105a2b415b826c02b8a498
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       gcr.io/google-samples/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 07 May 2026 12:43:22 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-zdbmb (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-zdbmb:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  7m6s   default-scheduler  Successfully assigned k8s-basics/kubernetes-bootcamp-978dd9cbc-gsxbl to k8s-101-control-plane
  Normal  Pulling    7m5s   kubelet            spec.containers{kubernetes-bootcamp}: Pulling image "gcr.io/google-samples/kubernetes-bootcamp:v1"
  Normal  Pulled     6m59s  kubelet            spec.containers{kubernetes-bootcamp}: Successfully pulled image "gcr.io/google-samples/kubernetes-bootcamp:v1" in 6.777s (6.777s including waiting). Image size: 83642968 bytes.
  Normal  Created    6m59s  kubelet            spec.containers{kubernetes-bootcamp}: Created container kubernetes-bootcamp
  Normal  Started    6m59s  kubelet            spec.containers{kubernetes-bootcamp}: Started container kubernetes-bootcamp

- Logs del contenedor disponibles.

Kubernetes Bootcamp App Started At: 2026-05-07T12:43:22.995Z | Running On:  kubernetes-bootcamp-978dd9cbc-gsxbl 

- Variables de entorno visibles por `exec`.

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=kubernetes-bootcamp-978dd9cbc-gsxbl
NPM_CONFIG_LOGLEVEL=info
NODE_VERSION=6.3.1
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
HOME=/root

## Errores comunes

- No usar `-n k8s-basics`.
- Ejecutar `exec` antes de que el pod este en `Running`.

## Reto

Abrir shell en el pod y hacer `curl http://localhost:8080`.

## Solucion del reto

```bash
kubectl -n k8s-basics exec -ti "$POD_NAME" -- bash
curl http://localhost:8080
exit
```

## Navegacion del libro

- [Anterior](02-crear-deployment-con-manifiestos.md)
- [Siguiente](04-exponer-app-con-service.md)
