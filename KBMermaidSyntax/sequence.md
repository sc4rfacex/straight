# Diagramas de Secuencia en Mermaid

## Sintaxis Básica
Inicia con "sequenceDiagram". 
Participants: participant [Actor]. 
Mensajes: ->> para llamadas, -->> para respuestas. 
Bloques: alt [Condición] ... end.

- Del original: `participant U as Usuario`.

## Tipos de Flechas (Oficial + Original)
| Sintaxis | Tipo | Uso |
|----------|------|-----|
| `->>`  | Sólida | Llamada síncrona |
| `-->>`  | Punteada | Respuesta |
| `->>+` | Activación | Inicia proceso |
| `-->>-` | Desactivación | Termina proceso |
| `-x` | Con X | Llamada perdida |

## Elementos Avanzados (Oficial + Original)
- Loops: `loop [Label] ... end`
- Alternativas: `alt [Condición] ... else ... end`
- Opcionales: `opt [Condición] ... end`
- Paralelos: `par [Label] ... and ... end`
- Notas: `Note right of [Actor]: Text`

## Errores Más Comunes
- Participants no declarados primero.
- Flechas incorrectas (e.g., --> en lugar de ->>).
- Bloques alt sin end.
- Mensajes sin actor destino.
- Activaciones no balanceadas (+ sin -).
- Usar -->> para respuestas sin llamada previa.

## Patrones Anti-Error (❌ vs ✅)
- ❌ Mensajes antes de declarar actores → ✅ Declara todos los `participant` al inicio.
- ❌ `alt` sin `end` → ✅ Siempre cierra bloques (`alt/else/end`, `opt/end`, `par/and/end`).
- ❌ Mezclar `->>` y `-->` → ✅ Usa `->>`/`-->>` para secuencia, `-->` solo en flowcharts.
- ❌ Activación abierta `->>+` sin `-->>-` → ✅ Balancea activaciones para evitar solapamientos.
- Checklist: actores declarados, bloques cerrados, flechas consistentes, notas ubicadas junto al actor correcto.

## Ejemplos
### Simple
```mermaid
sequenceDiagram
    Alice->>Bob: Hello Bob, how are you?
    Bob-->>Alice: Great!
```

### Medio
```mermaid
sequenceDiagram
    participant Usuario
    participant Servidor
    Usuario->>Servidor: Enviar credenciales de login
    Servidor-->>Usuario: Respuesta (token de sesión)
```

### Complejo
```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as Frontend
    participant A as API
    participant DB as Database
    U->>+F: Solicita datos
    F->>+A: GET /api/data
    A->>+DB: SELECT * FROM table
    DB-->>-A: Resultados
    A-->>-F: JSON Response
    F-->>-U: Muestra datos
    loop Cada 5 segundos
        F->>A: Polling
    end
    alt Login exitoso
        A->>F: Token
    else Fallido
        A->>F: Error 401
    end
    Note over U,A: Nota sobre interacción
```

### Pagos con microservicios (Industria: Fintech)
```mermaid
sequenceDiagram
    participant C as Cliente
    participant P as Payment UI
    participant G as API Gateway
    participant S as Payment Service
    participant Q as Cola de Eventos
    participant R as Risk Engine
    C->>P: Inicia pago
    P->>+G: POST /payments
    G->>+S: Validar método
    S-->>-G: PaymentIntent creado
    G-->>-P: Token de pago
    par Scoreo
        S->>+R: Evaluar fraude
        R-->>-S: Score OK/Block
    and Encolado
        S->>Q: Emitir evento PaymentInitiated
    end
    alt Score OK
        S-->>P: Confirmación
    else Bloqueado
        S-->>P: Error 403 Fraude
    end
```

### Observabilidad - Alerta a resolución
```mermaid
sequenceDiagram
    participant M as Monitor
    participant A as Alertmanager
    participant O as On-call
    participant R as Runbook Bot
    M->>A: Enviar alerta (CPU > 90%)
    A->>O: Notificación Pager
    O-->>A: ACK
    opt Mitigación automática
        A->>R: Ejecutar script de escala
        R-->>A: Resultado OK
    end
    par Diagnóstico
        O->>M: Solicitar métricas
        M-->>O: Panel grafana
    and Corrección
        O->>R: Reiniciar servicio
        R-->>O: Éxito
    end
    A-->>M: Cerrar alerta
```

### Data & Analytics - ETL diario
```mermaid
sequenceDiagram
    participant S as Scheduler
    participant I as Ingestor
    participant T as Transformer
    participant W as Warehouse
    participant D as Dashboard
    S->>I: Trigger ingest 02:00
    I->>+W: Carga raw
    W-->>-I: ACK
    I->>+T: Emit evento "raw_ready"
    T->>W: Leer raw
    T-->>W: Escribir tabla limpia
    W-->>D: Refrescar materialized view
    D-->>S: Reporte éxito
    Note over T,W: Garantiza idempotencia y particionado
```

## Buenas Prácticas
- Declara participants al inicio.
- Usa activaciones para mostrar duración.
- Añade notas para aclaraciones.
- Incluye alternativas para ramificaciones; verifica cronología.
- Métricas: <20 interacciones; incluye errores.
- Troubleshooting: valida cierres de bloques, balancea activaciones y evita mezclar sintaxis de flowchart; usa Mermaid Live Editor para detectar flechas huérfanas.