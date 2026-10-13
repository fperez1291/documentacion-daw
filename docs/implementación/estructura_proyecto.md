---
sidebar_position: 4
---

# Implementación y Estructura

El proyecto backend de **TaskFlow Pro** está desarrollado en Python y sigue una arquitectura orientada a servicios (SOA) con separación clara de responsabilidades. Esta estructura está diseñada para ser escalable y mantenible en entornos de producción, facilitando la colaboración del equipo, el despliegue continuo y la realización de pruebas.

## Estructura del Repositorio

A continuación, se detalla la estructura del código fuente, que incluye la configuración para el despliegue con Docker, control de dependencias, migraciones de base de datos y flujos de CI/CD.

```text
taskflow-pro-backend/
├── .github/                   # Workflows de GitHub Actions (CI/CD)
│   └── workflows/
│       ├── test.yml           # Pipeline de pruebas automatizadas
│       └── deploy.yml         # Pipeline de despliegue a producción
├── app/                       # Código fuente de la aplicación principal
│   ├── api/                   # Controladores / Endpoints (Rutas de la API HTTP)
│   │   ├── v1/
│   │   │   ├── Facturas.py
│   │   │   ├── Tareas.py
│   │   │   └── Usuarios.py
│   │   └── Router.py          # Enrutador principal de la aplicación
│   ├── core/                  # Configuración global, seguridad y gestión de entorno
│   │   ├── Config.py
│   │   └── Security.py
│   ├── db/                    # Conexión a base de datos y migraciones (Ej. Alembic)
│   │   ├── migrations/
│   │   └── Session.py
│   ├── models/                # Entidades de Base de Datos (Clases ORM - Ej. SQLAlchemy)
│   │   ├── Cliente.py
│   │   ├── Tarea.py
│   │   └── Usuario.py
│   ├── schemas/               # Validadores de datos de entrada/salida (Ej. Pydantic)
│   │   ├── ClienteSchema.py
│   │   └── TareaSchema.py
│   ├── services/              # Capa de lógica de negocio pura
│   │   ├── FacturacionService.py
│   │   └── TareaService.py
│   └── utils/                 # Funciones auxiliares genéricas y utilidades
│       └── Logger.py
├── tests/                     # Pruebas automatizadas (Pytest)
│   ├── Conftest.py            # Configuración y fixtures compartidas para los tests
│   ├── integration/           # Pruebas de integración (Llamadas a la API con BD de prueba)
│   └── unit/                  # Pruebas unitarias aisladas (Tests de la capa services)
│       └── TestFacturacion.py
├── .env.example               # Plantilla de variables de entorno requeridas
├── .gitignore                 # Archivos y carpetas ignorados por Git
├── alembic.ini                # Archivo de configuración para migraciones de BD
├── docker-compose.yml         # Orquestación de contenedores (App + Base de Datos + Caché)
├── Dockerfile                 # Receta para construir la imagen Docker de la API
├── pyproject.toml             # Definición de dependencias modernas (Poetry / Pip)
└── README.md                  # Documentación de inicio rápido del repositorio
```

## Función Principal (Ejemplo de Implementación)

En esta arquitectura, los controladores de la capa `api/` se encargan únicamente de recibir peticiones HTTP y devolver respuestas. Toda la lógica de negocio se aísla de forma independiente en la capa `services/`, lo que permite que el código sea fácilmente testable.

A modo de ejemplo, mostramos el servicio de facturación (`app/services/FacturacionService.py`), el cual encapsula las reglas de negocio críticas para aplicar descuentos:

```python
# app/services/FacturacionService.py

def calcularPrecioFinal(precioBase: float, esVip: bool) -> float:
    """
    Calcula el precio final aplicando descuentos según las estrictas reglas de negocio.
    
    Reglas:
    - Si el precio base es menor o igual a 0, lanza una excepción (ValueError).
    - Los clientes VIP tienen un 20% de descuento automático.
    - Los clientes normales con presupuestos superiores a 100€ tienen un 10% de descuento.
    - En el resto de casos, no hay descuento aplicable.
    """
    if precioBase <= 0:
        raise ValueError("El precio base debe ser estrictamente positivo.")
    
    descuento = 0.0
    
    if esVip:
        descuento = 0.20
    elif precioBase > 100.0:
        descuento = 0.10
        
    precioFinal = precioBase * (1.0 - descuento)
    return round(precioFinal, 2)
```
