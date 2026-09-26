# Especificación de Requisitos del Sistema

Este documento describe los requisitos funcionales y no funcionales para la versión 1.0 de EcoTrack.

## Requisitos Funcionales (RF)

| Código    | Descripción                                                                                                                  | Prioridad |
| --------- | :--------------------------------------------------------------------------------------------------------------------------- | :-------- |
| **RF-01** | El sistema debe permitir el registro e inicio de sesión de usuarios autenticados mediante JWT.                               | Alta      |
| **RF-02** | El usuario podrá registrar lecturas de consumo indicando tipo de energía, valor numérico y fecha.                            | Alta      |
| **RF-03** | El sistema calculará automáticamente la huella de carbono equivalente ($kg\,CO_2e$) basada en el tipo de consumo registrado. | Alta      |
| **RF-04** | El usuario podrá visualizar un resumen gráfico de sus consumos por mes y tipo de suministro.                                 | Media     |
| **RF-05** | El sistema permitirá exportar informes mensuales en formato PDF.                                                             | Baja      |

## Requisitos No Funcionales (RNF)

| Código     | Descripción                                                                                                | Criterio de Aceptación                                         |
| :--------- | :--------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------- |
| **RNF-01** | **Rendimiento:** El tiempo de respuesta del servidor para consultas analíticas no superará los 2 segundos. | Pruebas de carga con $100$ usuarios concurrentes.              |
| **RNF-02** | **Seguridad:** Todas las contraseñas deben ser almacenadas utilizando el algoritmo de hashing `bcrypt`.    | Auditoría de código.                                           |
| **RNF-03** | **Disponibilidad:** El frontend del sistema estará alojado de forma estática con un SLA del $99.9\%$.      | Hospedaje en GitHub Pages.                                     |
| **RNF-04** | **Usabilidad:** La interfaz debe cumplir con el diseño responsivo (*Mobile First*).                        | Pruebas de visualización en dispositivos móviles y escritorio. |
