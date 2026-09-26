# Diseños del Sistema: Diagrama de Clases

A continuación se muestra el modelado de clases del dominio del sistema **EcoTrack**, representado mediante notación Mermaid.

```mermaid
classDiagram
    class Usuario {
        +Int id
        +String nombre
        +String email
        +String contrasenaHash
        +autenticar(String pass) Boolean
    }

    class Sede {
        +Int id
        +String direccion
        +String ciudad
        +obtenerConsumoTotal() Float
    }

    class RegistroConsumo {
        +Int id
        +DateTime fecha
        +Float valor
        +TipoConsumo tipo
        +calcularEmisionCO2() Float
    }

    class TipoConsumo {
        <<enumeration>>
        ELECTRICIDAD
        GAS_NATURAL
        AGUA
    }

    class FactorEmision {
        +Int id
        +TipoConsumo tipo
        +Float factorCO2PerUnidad
        +String unidadMedida
    }

    Usuario "1" -- "*" Sede : gestiona >
    Sede "1" -- "*" RegistroConsumo : contiene >
    RegistroConsumo "1" -- "1" TipoConsumo : pertenece a >
    FactorEmision "1" -- "1" TipoConsumo : aplica a >

```

## Descripción de Entidades

- **Usuario:** Representa al administrador o gestor dentro del sistema.
- **Sede:** Instalación o propiedad física perteneciente a la empresa a la que se le atribuyen consumos.
- **RegistroConsumo:** Entidad que almacena la cantidad de suministro utilizado en un periodo determinado.
- **FactorEmision:** Unidades de conversión configurables para calcular el impacto de carbono equivalente.
