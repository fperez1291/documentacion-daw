---
id: despliegue
title: Despliegue de TaskFlow Pro
sidebar_label: Despliegue de la aplicación
sidebar_position: 7
---

# Despliegue de TaskFlow Pro

El despliegue del backend de **TaskFlow Pro** está diseñado para ser automatizado, predecible y escalable. Utilizamos contenedores Docker para aislar el entorno de ejecución y GitHub Actions para nuestro flujo de Integración y Despliegue Continuo (CI/CD).

## Requisitos Previos

Para desplegar la aplicación en un entorno de producción o preproducción, el servidor destino debe contar con:

- **Docker Engine** (versión 24.x o superior).
- **Docker Compose** (versión 2.x o superior).
- Acceso a Internet para descargar las imágenes base y dependencias.
- Un proxy inverso (como Nginx o Traefik) configurado para gestionar los certificados SSL (HTTPS).

## Despliegue Manual con Docker Compose

El repositorio incluye un archivo `docker-compose.yml` que orquesta los servicios necesarios: la API principal (Python), la base de datos (PostgreSQL) y el sistema de caché (Redis).

Para un despliegue manual rápido, sigue estos pasos desde la raíz del proyecto:

1. **Configurar el entorno:** copia la plantilla de variables de entorno y ajusta las credenciales (URL de la base de datos, claves secretas, etc.).

   ```bash
   cp .env.example .env
   ```

2. **Construir y levantar los contenedores:** este comando compilará la imagen utilizando el `Dockerfile` y levantará los servicios en segundo plano.

   ```bash
   docker compose up --build -d
   ```

3. **Ejecutar migraciones de Base de Datos:** una vez los contenedores estén corriendo, aplica las migraciones de Alembic para crear las tablas necesarias.

   ```bash
   docker compose exec api alembic upgrade head
   ```

## Integración y Despliegue Continuo (CI/CD)

El ciclo de vida del código está completamente automatizado a través de GitHub Actions. Las configuraciones se encuentran en el directorio `.github/workflows/`.

### 1. Pipeline de Pruebas (`test.yml`)

Se ejecuta automáticamente en cada *Pull Request* o al hacer *push* a la rama principal (`main`).

- Configura un entorno Python limpio.
- Levanta una base de datos de pruebas efímera.
- Ejecuta los linters (ej. `flake8`, `black`).
- Ejecuta la suite de pruebas unitarias e integración con `pytest`.
- Si la cobertura de código es inferior al 80% o falla algún test, el despliegue se bloquea.

### 2. Pipeline de Despliegue (`deploy.yml`)

Se dispara únicamente cuando un código ha pasado las pruebas y se integra en la rama `main` o se crea una nueva *Release*.

- **Build:** Compila la nueva imagen Docker de la API.
- **Registry:** Sube la imagen etiquetada a GitHub Container Registry (GHCR) o Docker Hub.
- **Deploy:** Se conecta vía SSH al servidor de producción, actualiza la imagen a la última versión, ejecuta las migraciones de base de datos de forma segura y reinicia los contenedores sin tiempo de inactividad (*Zero Downtime Deployment*).

## Monitorización y Logs

Una vez desplegada, los logs de la aplicación pueden ser consultados directamente a través de Docker:

```bash
# Ver los logs en tiempo real del servicio de la API
docker compose logs -f api
```

Para entornos de producción reales, se recomienda configurar un stack de monitorización adicional (como Prometheus y Grafana) o enviar los logs centralizados a plataformas como ELK o Datadog.
