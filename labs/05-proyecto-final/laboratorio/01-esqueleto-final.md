# Step 01 - Esqueleto final

## Objetivo del step

Preparar la estructura final del proyecto para integrar API, ETL, Compose y Kubernetes en una entrega única.

## Fundamento del step

Este step no es solo ejecución de comandos: su objetivo es construir criterio técnico. Cada acción busca evitar errores frecuentes de entorno, de configuración o de integración entre servicios. Antes de avanzar, asegúrate de entender qué problema resuelve cada bloque.

## Ejecución guiada

### 1) Crear estructura principal

```bash
mkdir -p labs/05-proyecto-final/trabajo/proyecto-final/{api,etl,k8s/base,k8s/overlays/local}
```

### 2) Crear `README.md` del proyecto final

Crea `labs/05-proyecto-final/trabajo/proyecto-final/README.md`:

```markdown
# Proyecto final - Docker + Kubernetes

Este proyecto integra:

- API en contenedor
- ETL batch en contenedor
- Orquestación local con Docker Compose
- Despliegue en Kubernetes con `base` y `overlays`

## Estructura esperada

- `api/`
- `etl/`
- `docker-compose.yml`
- `k8s/base/`
- `k8s/overlays/local/`
```

**Qué estamos haciendo aquí**

- Documentar el alcance del proyecto desde el inicio.
- Definir una referencia común para alumno, profesor y revisión final.

### 3) Crear `docker-compose.yml` inicial

Crea `labs/05-proyecto-final/trabajo/proyecto-final/docker-compose.yml`:

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: analytics
      POSTGRES_PASSWORD: analytics
      POSTGRES_DB: analytics
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  api:
    build: ./api
    environment:
      DATABASE_URL: postgresql+psycopg://analytics:analytics@db:5432/analytics
    depends_on:
      - db
    ports:
      - "8000:8000"

  etl:
    build: ./etl
    environment:
      DATABASE_URL: postgresql+psycopg://analytics:analytics@db:5432/analytics
    depends_on:
      - db
    profiles:
      - batch

volumes:
  pgdata:
```

**Qué estamos haciendo aquí**

- Definir contrato mínimo de ejecución local para todo el sistema.
- Dejar ETL como proceso opcional (`profile`) para ejecutarlo bajo demanda.

### 4) Verificar árbol de trabajo

```bash
ls -R labs/05-proyecto-final/trabajo/proyecto-final
```

## Qué validas y qué debes ver

- Carpetas `api`, `etl`, `k8s/base`, `k8s/overlays/local` creadas.
- Archivos base presentes con contenido inicial para comenzar integración.

## Errores comunes

- Crear rutas fuera de `trabajo/proyecto-final`.
- Olvidar carpeta `overlays/local`.

## Reto

Añade carpeta `k8s/overlays/batch` para separar configuración de procesos batch.

## Solución del reto

```bash
mkdir -p labs/05-proyecto-final/trabajo/proyecto-final/k8s/overlays/batch
```

## Navegacion del libro

- [Anterior](../03-criterios-de-entrega.md)
- [Siguiente](02-despliegue-final.md)
