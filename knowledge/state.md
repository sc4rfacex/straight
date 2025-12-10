# Diagramas de Estado en Mermaid

Los diagramas `stateDiagram-v2` permiten modelar lifecycles, ramificaciones y paralelismos. Usa siempre la versión `-v2` para compatibilidad.

## Sintaxis y notas clave
- Inicio/Fin: `[*] --> Estado` y `Estado --> [*]`.
- Transiciones: `Origen --> Destino : evento/condición`.
- Compuestos: `state "Nombre" { ... }` para anidar.
- Paralelo: `<<fork>>` para dividir y `<<join>>` para sincronizar.
- Decisión: `<<choice>>` para ramas condicionales.

### Errores frecuentes
- Olvidar `stateDiagram-v2` o cerrar llaves de estados compuestos.
- Transiciones sin etiqueta que dificultan lectura.
- Dead-ends sin camino a fin o sin estados de error.
- Reutilizar el mismo alias con mayúsculas/minúsculas cambiadas.

## Ejemplos específicos

### Workflow de aprobación de contenido (empresarial)
```mermaid
stateDiagram-v2
    [*] --> Borrador
    Borrador --> Revision : enviar()
    Revision --> Aprobado : cumple_politicas
    Revision --> Rechazado : feedback
    Rechazado --> Borrador : ajustar()
    Aprobado --> Publicado : programar()
    Publicado --> [*]
```
Puntos clave: incluye loop de corrección y transición explícita a publicación.

### Pedido e-commerce con estados paralelos
```mermaid
stateDiagram-v2
    [*] --> Pago
    state Pago {
        [*] --> Procesando
        Procesando --> Autorizado : ok
        Procesando --> Error : rechazo
        Error --> Procesando : reintento
        Autorizado --> [*]
    }
    Pago --> Fullfilment
    state Fullfilment {
        [*] --> Preparando
        Preparando --> Enviado : etiqueta
        Enviado --> Entregado : recibido
        Enviado --> Devolucion : fallo
        Devolucion --> [*]
        Entregado --> [*]
    }
    Pago --> [*] : cancelado
```
Puntos clave: dos subprocesos paralelos (pago y entrega) con rutas de error.

### Ciclo de vida de dispositivo IoT
```mermaid
stateDiagram-v2
    [*] --> Offline
    Offline --> Conectando : boot
    Conectando --> Online : handshake_ok
    Conectando --> Error : timeout
    Online --> Actualizando : firmware()
    Actualizando --> Online : ok
    Actualizando --> Error : fallo_firmware
    Error --> Conectando : reintentar
    Online --> Mantenimiento : programado
    Mantenimiento --> Online : terminar
```
Puntos clave: incluye timeouts y reintentos automáticos.

### Personaje de videojuego con estados anidados
```mermaid
stateDiagram-v2
    [*] --> Activo
    state Activo {
        [*] --> Explorando
        Explorando --> Combate : enemigo
        Combate --> Explorando : victoria
        Combate --> KO : vida<=0
        state KO {
            [*] --> Caido
            Caido --> Reaparecer : respawn
            Reaparecer --> [*]
        }
    }
    Activo --> Pausa : menu
    Pausa --> Activo : resume
```
Puntos clave: estados anidados para KO y transición de pausa global.

### Sistema de aprobación de gastos con choice/join
```mermaid
stateDiagram-v2
    [*] --> Solicitud
    Solicitud --> Evaluacion : enviar
    state Evaluacion {
        [*] --> if_monto <<choice>>
        if_monto --> Gerencia : monto>500
        if_monto --> Finanzas : monto<=500
        Gerencia --> Join <<join>>
        Finanzas --> Join
        Join --> Resultado
    }
    Resultado --> Aprobado : ok
    Resultado --> Rechazado : observaciones
    Rechazado --> Solicitud : corregir
```
Puntos clave: ramas condicionales con `choice` y sincronización posterior.

### Proceso de CI/CD simplificado
```mermaid
stateDiagram-v2
    [*] --> Build
    Build --> Test : ok
    Build --> Fallo : error_build
    Test --> Deploy : passed
    Test --> Fallo : tests_red
    Deploy --> Monitoreo : release
    Monitoreo --> Rollback : alerta
    Monitoreo --> [*] : estable
    Rollback --> Build : fix
```
Puntos clave: incluye rollback y bucle de mejora continua.

## Buenas prácticas
- Siempre muestra un camino feliz y al menos un camino de error.
- Prefiere nombres de estados verbales/claros y eventos en minúsculas con guiones bajos.
- Limita la profundidad de anidación a 2-3 niveles para mantener legibilidad.
- Verifica que cada fork tenga un join o un estado terminal claro.
