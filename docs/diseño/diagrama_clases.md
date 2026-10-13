---
id: diagrama-clases
title: Diseño de TaskFlow Pro
sidebar_label: Diagrama de Clases
sidebar_position: 4
---

# Diseño del Sistema

A continuación se presenta el diagrama de clases completo de la plataforma TaskFlow Pro, modelando las entidades principales del dominio, sus atributos, métodos y las relaciones entre ellas. Se utiliza la sintaxis de Mermaid para su renderizado.

```mermaid
---
title: Diagrama TaskFlow Pro
config:
  theme: default
  look: classic
  layout: dagre
---
classDiagram
    class Rol {
        <<enumeration>>
        ADMINISTRADOR
        MANAGER
        EMPLEADO
    }

    class EstadoTarea {
        <<enumeration>>
        PENDIENTE
        EN_PROGRESO
        EN_REVISION
        COMPLETADA
    }

    class Usuario {
        + int id
        + String nombre
        + String email
        + String passwordHash
        + Rol rol

        + iniciarSesion() bool
        + cerrarSesion() void
        + actualizarPerfil(datos: Map) bool
    }

    class Cliente {
        + int id
        + String nombreEmpresa
        + String emailContacto
        + bool esVip
        + String nif

        + getDescuentoAplicable() float
    }

    class Proyecto {
        + int id
        + String nombre
        + String descripcion
        + Date fechaInicio
        + Date fechaFinEstimada
        + String estado

        + añadirTarea(tarea: Tarea) void
        + calcularPresupuestoTotal() float
        + generarFactura() Factura
    }

    class Tarea {
        + int id
        + String titulo
        + String descripcion
        + EstadoTarea estado
        + float horasEstimadas
        + float horasReales
        + float precioBase

        + asignarUsuario(usuario: Usuario) void
        + registrarTiempo(horas: float) void
        + completarTarea() void
    }

    class Factura {
        + int id
        + String numeroFactura
        + Date fechaEmision
        + float importeBase
        + float impuestos
        + float importeTotal
        + String estadoPago

        + calcularTotal() float
        + exportarPDF() String
    }

    class FacturacionService {
        <<service>>
        + calcularPrecioFinal(precioBase: float, esVip: bool) float
        + aplicarImpuestos(importe: float, porcentaje: float) float
    }

    %% Relaciones
    Usuario  "*" --  "1"    Rol : tiene asignado
    Cliente  "1" *-- "*"    Proyecto : encarga
    Proyecto "1" *-- "*"    Tarea : se compone de
    Tarea    "*" --> "0..1" Usuario : asignada a
    Tarea    "*" --> "1"    EstadoTarea : tiene
    Proyecto "1" --  "*"    Factura : genera
    Factura  "*" --> "1"    FacturacionService : utiliza para cálculo
```

## Descripción de las Entidades

- **Usuario y Rol:** Define a los empleados del sistema y su nivel de acceso. Un usuario puede ser asignado a múltiples tareas.
- **Cliente:** Representa a las empresas o particulares que contratan los proyectos. Incluye el flag `esVip` que el servicio de facturación utiliza para aplicar descuentos.
- **Proyecto:** Agrupa múltiples tareas y pertenece a un único cliente. Es la entidad desde la cual se puede calcular un presupuesto global o emitir una factura.
- **Tarea:** La unidad mínima de trabajo. Registra estimaciones, horas reales invertidas y un precio base.
- **Factura y FacturacionService:** Manejan la lógica de negocio para calcular importes finales, aplicar reglas de descuentos, impuestos y emitir documentos en PDF.
