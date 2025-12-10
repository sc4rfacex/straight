# Diagramas C4 Component en Mermaid

Los diagramas `C4Component` (nivel 3) detallan la estructura interna de un contenedor: controladores, servicios, adaptadores y cómo colaboran.

## Sintaxis básica
- Encabezado: `C4Component`.
- Contenedor padre (opcional pero recomendado): `Container_Boundary(id, "Nombre") { ... }` para agrupar componentes.
- Componentes: `Component(alias, "Etiqueta", "Tecnología", "Responsabilidad")` y `ComponentDb(alias, "Etiqueta", "Tecnología", "Responsabilidad")`.
- Sistemas externos y contenedores adyacentes: `System_Ext` o `Container` según corresponda para dejar claro el contexto.
- Relaciones: `Rel(origen, destino, "Tipo", "Protocolo")`.

## Errores comunes
- No declarar el contenedor padre, dejando componentes flotando sin contexto.
- Reutilizar alias de contenedores en componentes, generando colisiones.
- Falta de responsabilidades o tecnología en componentes, impidiendo distinguir roles.
- Relaciones sin protocolo que dificultan validar dependencias.

## Ejemplos

### API REST modular (baseline)
```mermaid
C4Component
    title API Pedidos
    Container_Boundary(api, "Backend Pedidos") {
        Component(controller, "OrderController", "NestJS", "Expone endpoints")
        Component(service, "OrderService", "TypeScript", "Reglas de negocio")
        Component(repo, "OrderRepository", "TypeORM", "Persistencia")
        ComponentDb(db, "Orders DB", "PostgreSQL", "Pedidos y líneas")
    }
    System_Ext(psp, "Pasarela", "Cobros")
    Rel(controller, service, "Invoca", "Nest DI")
    Rel(service, repo, "Usa", "ORM")
    Rel(repo, db, "SQL", "TCP")
    Rel(service, psp, "Procesa pago", "HTTPS")
```

### Microservicio de pagos con antifraude
```mermaid
C4Component
    title Pago y antifraude
    Container_Boundary(pay, "Pago") {
        Component(api, "Payment API", "Spring WebFlux", "Recibe solicitudes")
        Component(orchestrator, "Orquestador", "Java", "Dirige flujo de pago")
        Component(riskClient, "Cliente Riesgo", "gRPC", "Consulta scoring")
        Component(notifier, "Notifier", "Kotlin", "Envía eventos")
        ComponentDb(txDb, "Transacciones", "PostgreSQL", "Pagos y estados")
    }
    System_Ext(risk, "Servicio Riesgo", "Score fraude")
    System_Ext(psp, "PSP", "Procesa tarjeta")
    Rel(api, orchestrator, "Valida y delega", "Reactive")
    Rel(orchestrator, riskClient, "Solicita score", "gRPC")
    Rel(orchestrator, psp, "Autoriza", "HTTPS")
    Rel(orchestrator, notifier, "Emite evento", "AMQP")
    Rel(orchestrator, txDb, "Guarda estado", "SQL")
```

### Plataforma de mensajería con colas y websockets
```mermaid
C4Component
    title Mensajería en tiempo real
    Container_Boundary(chat, "Chat Service") {
        Component(gateway, "WebSocket Gateway", "Node.js", "Conexiones clientes")
        Component(auth, "Auth Adapter", "Node", "Validar tokens")
        Component(router, "Message Router", "Go", "Fan-out mensajes")
        Component(history, "History Writer", "Go", "Persistir conversaciones")
        ComponentDb(db, "Chat DB", "Cassandra", "Mensajes e índices")
    }
    System_Ext(users, "Usuarios", "Clientes web/móvil")
    Container(queue, "Bus", "NATS", "Eventos chat")
    Rel(users, gateway, "Conecta", "WebSocket")
    Rel(gateway, auth, "Verifica token", "JWT")
    Rel(gateway, router, "Envía mensaje", "NATS")
    Rel(router, queue, "Publica", "NATS")
    Rel(queue, history, "Consume", "NATS stream")
    Rel(history, db, "Escribe", "CQL")
```

### Frontend modular con micro-frontend
```mermaid
C4Component
    title Portal B2B
    Container_Boundary(front, "Frontend") {
        Component(shell, "App Shell", "React", "Layout y routing")
        Component(authMf, "Microfrontend Auth", "React", "Login y sesión")
        Component(ordersMf, "Microfrontend Órdenes", "React", "Gestión pedidos")
        Component(analyticsMf, "Microfrontend Analytics", "Vue", "KPIs")
    }
    Container(api, "API Gateway", "Kong")
    Rel(shell, authMf, "Carga", "Module Federation")
    Rel(shell, ordersMf, "Carga", "Module Federation")
    Rel(shell, analyticsMf, "Carga", "Module Federation")
    Rel(ordersMf, api, "Consulta", "HTTPS/REST")
    Rel(analyticsMf, api, "Consulta", "HTTPS/REST")
```

### Clean Architecture para backend de catálogo
```mermaid
C4Component
    title Catálogo con capas limpias
    Container_Boundary(catalog, "Catálogo") {
        Component(controller, "Controller", "FastAPI", "Adaptador HTTP")
        Component(useCase, "Use Case", "Python", "Orquesta reglas de producto")
        Component(domain, "Dominio", "Entidad", "Reglas de negocio puras")
        Component(repoPort, "Repo Port", "Interface", "Contrato de persistencia")
        Component(repoAdapter, "Repo Adapter", "SQLAlchemy", "Implementación DB")
        ComponentDb(db, "Catálogo DB", "PostgreSQL", "Productos y variantes")
    }
    System_Ext(search, "Buscador", "Indexación")
    Rel(controller, useCase, "Invoca", "DI")
    Rel(useCase, domain, "Usa", "in-memory")
    Rel(useCase, repoPort, "Define contrato", "interface")
    Rel(repoAdapter, repoPort, "Implementa", "extends")
    Rel(repoAdapter, db, "SQL", "TCP")
    Rel(useCase, search, "Publica evento", "HTTP webhook")
```

### Observabilidad y tracing del servicio
```mermaid
C4Component
    title Observabilidad servicio API
    Container_Boundary(obs, "Instrumentation") {
        Component(logger, "Logger", "Go", "Estructura logs")
        Component(tracer, "Tracer", "OpenTelemetry", "Genera spans")
        Component(metrics, "Metrics", "Prometheus SDK", "Exposición de métricas")
    }
    Container(api, "Servicio API", "Go", "Negocio")
    System_Ext(stack, "Stack Observabilidad", "Grafana Cloud")
    Rel(api, logger, "Envía", "gRPC/OTLP")
    Rel(api, tracer, "Propaga context", "W3C Trace Context")
    Rel(api, metrics, "Expone", "/metrics")
    Rel(logger, stack, "Logs", "OTLP/HTTP")
    Rel(tracer, stack, "Spans", "OTLP/gRPC")
    Rel(metrics, stack, "Métricas", "Prometheus remote write")
```

## Buenas prácticas
- Mantén la vista dentro de un contenedor: usa límites claros para evitar mezclar niveles.
- Explica responsabilidad y tecnología en cada componente; evita nombres genéricos.
- Incluye adaptadores (web, colas, batch) y puertos (interfaces) para mostrar dependencias explícitas.
- Muestra dependencias externas (PSP, búsqueda, observabilidad) con `System_Ext` o `Container` para dejar claro el contexto.
- Valida en Mermaid Live Editor: cada `Rel` necesita etiqueta y protocolo para legibilidad.
