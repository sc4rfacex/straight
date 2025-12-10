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

## Patrones Anti-Error (❌ vs ✅)
- ❌ `flowchart TD\nA-->B` (sin espacios, difícil de leer) → ✅ `flowchart TD\n    A --> B` (sangría consistente).
- ❌ `A-->B-->` (flecha huérfana) → ✅ `A --> B` (si no hay destino, convierte en nota o elimina).
- ❌ `subgraph Zona` sin `end` → ✅ `subgraph Zona\n    ...\nend` (valida subgrafos balanceados).
- ❌ `A -- Texto B` (texto sin delimitadores) → ✅ `A -- Texto --> B` (usa `-->` o `---`).
- Checklist rápido: dirección definida, llaves/corchetes balanceados, subgrafos cerrados, flechas con origen y destino existentes.

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

### Operaciones - Respuesta a incidentes (Industria: Observabilidad)
```mermaid
flowchart LR
    subgraph Alerting
        A[Alerta de CPU Alta]
        B[PagerDuty]
    end
    subgraph Respuesta
        C{¿Autoescalar disponible?}
        D[Crear Ticket]
        E[Escalar Pods]
        F[Ejecutar Runbook]
    end
    subgraph Postmortem
        G[Recolección Logs]
        H[Análisis RCA]
    end
    A --> B --> C
    C -->|Sí| E --> F
    C -->|No| D --> F --> G --> H
    style C fill:#fff3cd,stroke:#f0ad4e
    style H fill:#d9edf7,stroke:#31708f
```

### Retail Omnicanal (Industria: E-commerce)
```mermaid
flowchart TD
    subgraph Canales
        Web[Checkout Web]
        POS[Punto de Venta]
        App[App Móvil]
    end
    subgraph Procesos
        P1{¿Stock disponible?}
        P2[Separar inventario]
        P3[Confirmar pago]
        P4[Orquestar envío]
    end
    subgraph Logística
        L1[Almacén Principal]
        L2[Dark Store]
        L3[Transportista]
    end
    Web --> P1
    POS --> P1
    App --> P1
    P1 -->|Sí| P2 --> P3 --> P4 --> L3
    P1 -->|No| L2
    L1 -.-> P2
    L2 -.-> P4
    style P1 fill:#fff3cd,stroke:#f0ad4e
    style P4 fill:#d9edf7,stroke:#31708f
```

### Salud - Gestión de citas (Industria: Healthcare)
```mermaid
flowchart LR
    Paciente([Paciente]) --> Portal[Portal Web]
    Portal --> Cita{¿Especialista disponible?}
    Cita -->|Sí| Agenda[Agenda cita]
    Cita -->|No| Lista[Lista de espera]
    Agenda --> Recordatorio[SMS/Email Recordatorio]
    Recordatorio --> Consulta[Consulta Médica]
    Consulta --> Pago[Procesar pago]
    Pago --> Encuesta[Encuesta de satisfacción]
    style Cita fill:#fff3cd,stroke:#f0ad4e
    style Pago fill:#dff0d8,stroke:#3c763d
```

## Buenas Prácticas

- Comienza simple, añade complejidad si necesario.
- Usa etiquetas descriptivas en nodos/conexiones.
- Aplica estilos (colores, iconos) para claridad.
- Verifica conexiones lógicas.
- Usa subgraphs para agrupar componentes; considera audiencia para detalle.
- Métricas: <10 nodos para simplicidad; complejidad ciclomatica <5.
- Troubleshooting: valida con Mermaid Live Editor, busca flechas huérfanas y subgrafos sin `end`, confirma que cada decisión tiene salidas etiquetadas.