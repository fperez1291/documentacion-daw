# Despliegue de la Documentación en GitHub Pages

La documentación de **EcoTrack** se despliega automáticamente en GitHub Pages mediante una acción integrada de GitHub Actions cada vez que se realiza un *push* a la rama `main`.

## Requisitos Previos

1. Tener habilitada la opción de **GitHub Pages** en la configuración del repositorio (`Settings > Pages`).
2. Configurar el origen de compilación en **GitHub Actions**.

## Flujo de Trabajo (CI/CD)

El archivo `.github/workflows/deploy-docs.yml` define el proceso automatizado:

```yaml
name: Deploy Documentation

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout del código
        uses: actions/checkout@v4

      - name: Configurar Pages
        uses: actions/configure-pages@v4

      - name: Cargar artefactos de documentación
        uses: actions/upload-pages-artifact@v3
        with:
          path: './docs'

      - name: Desplegar a GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```

## Verificación del Despliegue

Una vez completado el flujo de trabajo:

1. Dirígete a la pestaña **Actions** en el repositorio.
2. Comprueba que el *job* `Deploy Documentation` haya finalizado con éxito (icono verde).
3. Accede al sitio publicado en la URL asignada por GitHub (ejemplo: `https://<usuario>.github.io/ecotrack/`).
