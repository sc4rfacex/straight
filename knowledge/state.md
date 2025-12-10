# Diagramas de Estado en Mermaid

## Sintaxis Básica
Inicia con "stateDiagram-v2". 
Estados: [*] --> [Estado]. 
Transiciones: --> : "etiqueta". 
Compuestos: state "Grupo" { ... }.

- Del original: Usa -v2 para nuevo renderer.

## Estados Especiales (Oficial + Original)
- `[*]` Inicio/Fin.
- `<<choice>>` Decisión.
- `<<fork>>` Paralelización.
- `<<join>>` Sincronización.

## Errores Más Comunes
- Olvidar -v2 en keyword.
- Transiciones sin etiqueta.
- Estados sin [*] para inicio/fin.
- Compuestos sin cierre.
- Transiciones con dead-ends.

## Ejemplos
### Simple (Oficial)
```mermaid
stateDiagram-v2
    [*] --> Pendiente
    Pendiente --> Aprobado: Validar
```

### Medio (Del Original)
```mermaid
stateDiagram-v2
    [*] --> Inactivo
    Inactivo --> Activo : activar()
    Activo --> Procesando : iniciar()
    Procesando --> Completado : finish()
    Procesando --> Error : error()
    Error --> Activo : reintentar()
    Completado --> [*]
```

### Complejo (Del Original + Oficial)
```mermaid
stateDiagram-v2
    [*] --> Estado1
    Estado1 --> [*]
    state if_state <<choice>>
    [*] --> if_state
    if_state --> Estado2 : if condition
    if_state --> Estado3 : else
    state fork_state <<fork>>
    [*] --> fork_state
    fork_state --> Estado4
    fork_state --> Estado5
    state join_state <<join>>
    Estado4 --> join_state
    Estado5 --> join_state
    join_state --> [*]
    state Proceso {
        [*] --> Paso1
        Paso1 --> Paso2
    }
```

## Buenas Prácticas
- Cubre todos paths sin dead-ends.
- Incluye estados de error.
- Usa compuestos para lógica compleja.
- Métricas: <8 estados; transiciones lógicas 100%.