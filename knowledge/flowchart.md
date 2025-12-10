# Diagramas de Flujo (Flowcharts) en Mermaid

## Sintaxis Básica
Se definen con "flowchart" seguido de la dirección (e.g., LR para izquierda-derecha, TB para arriba-abajo). Nodos: [Etiqueta] para rectángulos, {Etiqueta} para decisiones. Conexiones: --> para flechas, -- Etiqueta --> para etiquetadas.

- Del documento original: Usa TD/LR/BT/RL para dirección.
- Oficial: `graph` es alias de `flowchart`.

## Tipos de Nodos (Oficial + Complemento)
| Sintaxis | Resultado | Uso |
|----------|-----------|-----|
| `[Texto]` | Rectángulo | Proceso estándar |
| `([Texto])` | Extremos redondeados | Inicio/Fin |
| `{Texto}` | Rombo | Decisión |
| `{{Texto}}` | Hexágono | Preparación |
| `[(Texto)]` | Cilindro | Base de datos |
| `((Texto))` | Círculo | Conector |
| `[/Texto/]` | Paralelogramo | Entrada/Salida |
| `[Texto]` | Trapecio | Manual |
| `A@{ shape: rect, label: "Text" }` (v11.3+) | Formas expandibles | Personalizado (e.g., circle, diamond) |

## Tipos de Conexiones (Oficial + Original)
- `A --> B`  – Flecha sólida
- `A --- B`  – Línea sin flecha
- `A -.-> B` – Flecha punteada
- `A ==> B`  – Flecha gruesa
- `A -->|texto| B` – Con etiqueta
- `A o--o B` – Círculo en extremos
- `A x--x B` – Cruz en extremos

## Subgrafos (Del Original + Oficial)
Usa `subgraph [Título]` para agrupar, con `end` para cerrar.

## Errores Más Comunes
- Indentación inconsistente: Debe ser uniforme (4 espacios).
- Dirección no especificada: Default TD, pero explícita es mejor.
- Nodos sin cierre: Olvidar ] o }.
- Flechas sin conexión clara: A --> B no definido.
- "end" en minúsculas: Usa "End" o workaround.
- Especiales como "o" o "x" primero: Añade espacio o capitaliza.
- Del PDF: Subgraphs sin end; iconos no cerrados.

## Ejemplos
### Simple
```mermaid
flowchart TD
    A([Inicio]) --> B[Proceso]
    B --> C{¿Condición?}
    C -->|Sí| D[Acción Sí]
    C -->|No| E[Acción No]
    D --> F([Fin])
    E --> F
```

### Medio
```mermaid
flowchart LR
    A[Inicio] --> B{¿Condición?}
    B -- Sí --> C[Opción 1]
    B -- No --> D[Opción 2]
    C --> E[Fin]
    D --> E[Fin]
```

### Complejo
```mermaid
flowchart TB
    subgraph Cliente
        U[Usuario]
    end
    subgraph DMZ ["Zona Desmilitarizada"]
        F[[Firewall]]
    end
    subgraph Backend
        A[(Servidor API)]
        B[(Base de Datos)]
    end
    U --> F --> A --> B
    style F fill:#f00,stroke:#333
    %% Rendimiento: Minimiza ramificaciones; seguridad: Incluye firewalls.
```

## Buenas Prácticas

- Comienza simple, añade complejidad si necesario.
- Usa etiquetas descriptivas en nodos/conexiones.
- Aplica estilos (colores, iconos) para claridad.
- Verifica conexiones lógicas.
- Usa subgraphs para agrupar componentes; considera audiencia para detalle.
- Métricas: <10 nodos para simplicidad; complejidad ciclomatica <5.