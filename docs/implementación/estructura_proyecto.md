---
id: estructura-proyecto
title: Estructura del Proyecto
sidebar_label: Estructura del Proyecto
sidebar_position: 4
---

# Estructura del Proyecto

El repositorio de **EcoTrack** está organizado mediante una arquitectura monorepo simplificada que separa el código fuente, la configuración de despliegue y la documentación oficial.

```text
ecotrack/
├── docs/
│   ├── analisis/
│   │   └── requisitos.md
│   ├── despliegue/
│   │   └── github_pages.md
│   ├── diseño/
│   │   └── diagrama_clases.md
│   ├── implementacion/
│   │   └── estructura_proyecto.md
│   ├── pruebas/
│   │   └── analisis_caja_blanca.md
│   └── intro.md
├── src/
│   ├── components/
│   │   ├── Dashboard.jsx
│   │   └── MetricCard.jsx
│   ├── services/
│   │   ├── api.js
│   │   └── carbonCalculator.js
│   ├── App.jsx
│   └── main.jsx
├── tests/
│   └── carbonCalculator.test.js
├── .github/
│   └── workflows/
│       └── deploy-docs.yml
├── package.json
└── README.md

```

## Convenciones de Directorios

- `docs/`: Archivos en formato Markdown procesados dinámicamente o desplegados como sitio estático.
- `src/components/`: Componentes reutilizables de la interfaz de usuario en React.
- `src/services/`: Capa de lógica de negocio pura y comunicación con APIS REST externas.
- `tests/`: Suite de pruebas unitarias e integración escritas con Vitest/Jest.
