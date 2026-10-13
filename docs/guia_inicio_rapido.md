---
sidebar_position: 2
---

# Guía de Inicio Rápido

Esta guía te ayudará a configurar el entorno de desarrollo local para **TaskFlow Pro** en pocos minutos. Está orientada a desarrolladores que necesiten levantar el proyecto en su máquina para contribuir al código.

## Requisitos previos

Antes de comenzar, asegúrate de tener instalado el siguiente software en tu equipo:

* **Python 3.10+**
* **Git**
* **Docker y Docker Compose** (opcional, para levantar la base de datos fácilmente)
* **Poetry** (gestor de dependencias)

## Instalación paso a paso

### 1. Clonar el repositorio

Clona el repositorio oficial en tu máquina local y accede al directorio del proyecto:

```bash
git clone [https://github.com/usuario/taskflow-pro-backend.git](https://github.com/usuario/taskflow-pro-backend.git)
cd taskflow-pro-backend
```

### 2. Configurar variables de entorno

Copia el archivo de ejemplo para crear tu configuración local. Ajusta los valores si es necesario (por ejemplo, credenciales de desarrollo).

```bash
cp .env.example .env
```

### 3. Instalar dependencias

Utilizamos Poetry para aislar y gestionar las dependencias del proyecto. Ejecuta el siguiente comando para crear el entorno virtual e instalar los paquetes necesarios:

```bash
poetry install
```

### 4. Levantar la base de datos (Opcional)

Si no tienes una instancia local de PostgreSQL, puedes usar Docker Compose para levantar una base de datos efímera para desarrollo:

```bash
docker compose up -d db
```

### 5. Ejecutar migraciones

Aplica las migraciones de Alembic para crear las tablas y esquemas necesarios en tu base de datos:

```bash
poetry run alembic upgrade head
```

### 6. Iniciar el servidor de desarrollo

Levanta el servidor local con recarga automática para reflejar los cambios en tiempo real:

```bash
poetry run uvicorn app.api.router:app --reload --host 127.0.0.1 --port 8000
```

¡Listo! La API estará disponible en `http://127.0.0.1:8000`. Puedes consultar la documentación interactiva de Swagger accediendo a `http://127.0.0.1:8000/docs`.
