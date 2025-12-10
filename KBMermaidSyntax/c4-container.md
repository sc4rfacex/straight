# Diagramas C4 Container en Mermaid

Los diagramas `C4Container` muestran las aplicaciones y bases de datos que conforman un sistema (nivel 2) y cómo se comunican.

## Sintaxis básica
- Encabezado: `C4Container`.
- Personas y sistemas externos: `Person(alias, "Etiqueta", "Descripción")`, `System_Ext(alias, "Etiqueta", "Descripción")`.
- Contenedores: `Container(alias, "Etiqueta", "Tecnología", "Responsabilidad")` y `ContainerDb(alias, "Etiqueta", "Tecnología", "Responsabilidad")`.
- Límites: `System_Boundary` o `Container_Boundary(id, "Nombre") { ... }` para agrupar contenedores relacionados.
- Relaciones: `Rel(origen, destino, "Tipo", "Protocolo")`.

## Errores comunes
- Dejar contenedores sin especificar tecnología/responsabilidad.
- Olvidar agrupar en límites cuando hay más de un contenedor por sistema.
- Etiquetas de relación vacías o sin protocolo (dificulta lectura).
- Reutilizar el mismo alias en distintos límites.

## Ejemplos

### Básico: SPA y API
```mermaid
C4Container
    title Sitio público
    Person(visitor, "Visitante")
    Container_Boundary(site, "Portal Web") {
        Container(spa, "Frontend", "React", "Experiencia de usuario")
        Container(api, "API", "Node.js", "Servicios públicos")
        ContainerDb(db, "Contenido", "PostgreSQL", "Páginas y medios")
    }
    Rel(visitor, spa, "Navega", "HTTPS")
    Rel(spa, api, "Consume", "HTTPS/JSON")
    Rel(api, db, "Lee/Escribe", "SQL")
```

### E-commerce en microservicios con límites claros
```mermaid
C4Container
    title Tienda online
    Person(customer, "Cliente")
    System_Ext(psp, "Pasarela de pago", "Procesa tarjetas")
    System_Ext(courier, "Courier", "Entrega paquetes")
    Container_Boundary(shop, "E-commerce") {
        Container(web, "Web", "Next.js", "Catálogo y carrito")
        Container(mobile, "App Móvil", "Flutter", "Compras móviles")
        Container(api, "API Gateway", "Kong", "Entrada unificada")
        Container(order, "Servicio de Pedidos", "Java/Spring", "Orquestar órdenes")
        Container(payment, "Servicio de Pagos", "Go", "Integrar PSP y antifraude")
        Container(inventory, "Inventario", "Python/FastAPI", "Stock en tiempo real")
        Container(messageBus, "Bus de eventos", "Kafka", "Integración async")
        ContainerDb(dbOrders, "DB Pedidos", "PostgreSQL", "Órdenes y pagos")
        ContainerDb(dbCatalog, "DB Catálogo", "MongoDB", "Productos y precios")
        Container(cache, "Cache", "Redis", "Sesiones y catálogos calientes")
    }
    Rel(customer, web, "Compra", "HTTPS")
    Rel(customer, mobile, "Compra", "HTTPS")
    Rel(web, api, "Llama", "HTTPS/REST")
    Rel(mobile, api, "Llama", "HTTPS/REST")
    Rel(api, order, "Enruta", "gRPC")
    Rel(api, payment, "Enruta", "gRPC")
    Rel(order, messageBus, "Publica eventos", "Kafka")
    Rel(order, inventory, "Consulta stock", "REST")
    Rel(order, dbOrders, "Guarda", "SQL")
    Rel(order, cache, "Lee", "TCP")
    Rel(payment, psp, "Procesa pago", "HTTPS")
    Rel(inventory, dbCatalog, "Lee/Escribe", "Mongo protocol")
    Rel(order, courier, "Envía guía", "HTTPS")
```

### Core bancario con zonas seguras
```mermaid
C4Container
    title Plataforma bancaria
    Person(customer, "Cliente Digital")
    System_Ext(regulator, "Regulador", "Reportes AML")
    Container_Boundary(dmz, "DMZ") {
        Container(edge, "API Edge", "NGINX", "Terminar TLS")
    }
    Container_Boundary(app, "Servicios Bancarios") {
        Container(auth, "Autenticación", "Keycloak", "OIDC y MFA")
        Container(accounts, "Cuentas", "Java/Spring", "Saldos y movimientos")
        Container(payments, "Pagos", ".NET", "Transferencias")
        Container(reporting, "Reporting", "Python", "Regulatorio")
        Container(message, "Bus interno", "RabbitMQ", "Eventos financieros")
        ContainerDb(ledger, "Ledger", "Oracle", "Asientos contables")
        ContainerDb(audit, "Auditoría", "PostgreSQL", "Trazas")
    }
    Rel(customer, edge, "API", "HTTPS")
    Rel(edge, auth, "Delegar auth", "OpenID")
    Rel(edge, accounts, "Proxy", "mTLS")
    Rel(edge, payments, "Proxy", "mTLS")
    Rel(auth, accounts, "Validar sesión", "JWT")
    Rel(payments, message, "Emite evento", "AMQP")
    Rel(accounts, ledger, "Asienta", "SQL")
    Rel(payments, ledger, "Debita/credita", "SQL")
    Rel(reporting, audit, "Guarda logs", "SQL")
    Rel(reporting, regulator, "Reporta", "SFTP")
```

### Arquitectura serverless orientada a datos
```mermaid
C4Container
    title Analytics serverless
    Person(analyst, "Analista")
    System_Ext(crm, "CRM", "Fuente de leads")
    Container_Boundary(serverless, "Pipeline") {
        Container(ingest, "Ingesta", "AWS Lambda", "Normaliza eventos")
        Container(queue, "Cola", "SQS", "Desacoplar fuentes")
        Container(stream, "Procesamiento", "Kinesis Analytics", "Enriquecer flujos")
        Container(warehouse, "Data Warehouse", "Snowflake", "Modelo analítico")
        Container(viz, "BI", "Metabase", "Dashboards")
        Container(storage, "Data Lake", "S3", "Raw/curated")
    }
    Rel(crm, queue, "Publica lead", "HTTPS/SQS")
    Rel(queue, ingest, "Dispara", "event")
    Rel(ingest, storage, "Guarda raw", "S3 API")
    Rel(ingest, stream, "Envía limpio", "Kinesis")
    Rel(stream, warehouse, "Carga", "Snowpipe")
    Rel(analyst, viz, "Consulta", "HTTPS")
    Rel(viz, warehouse, "Query", "SQL/ODBC")
```

### Telemedicina multicanal
```mermaid
C4Container
    title Plataforma de telemedicina
    Person(patient, "Paciente")
    Person(doctor, "Médico")
    System_Ext(pharmacy, "Farmacia", "Dispensa recetas")
    Container_Boundary(care, "Atención") {
        Container(portal, "Portal Web", "Vue", "Citas y chat")
        Container(mobile, "App Móvil", "React Native", "Consultas video")
        Container(video, "Servicio Video", "WebRTC SFU", "Videollamadas seguras")
        Container(records, "Historia Clínica", "Java", "EHR y notas")
        Container(prescriptions, "Recetas", "Python", "Firma y envíos")
        ContainerDb(dbRecords, "DB Clínica", "PostgreSQL", "Datos clínicos")
        Container(cache, "Cache", "Redis", "Sesiones")
    }
    Rel(patient, portal, "Agenda", "HTTPS")
    Rel(patient, mobile, "Consulta", "HTTPS")
    Rel(doctor, portal, "Revisa agenda", "HTTPS")
    Rel(portal, video, "Negocia sesión", "HTTPS")
    Rel(mobile, video, "Media", "WebRTC")
    Rel(doctor, records, "Actualiza", "HTTPS")
    Rel(records, dbRecords, "Lee/Escribe", "SQL")
    Rel(prescriptions, pharmacy, "Envía receta", "API")
    Rel(portal, cache, "Sesiones", "TCP")
```

### Observabilidad y seguridad agregadas
```mermaid
C4Container
    title Monitoreo centralizado
    Person(sre, "SRE")
    System_Ext(apps, "Aplicaciones", "Servicios productivos")
    Container_Boundary(obs, "Plataforma Observabilidad") {
        Container(collector, "Collector", "OpenTelemetry", "Recibe trazas y métricas")
        Container(queue, "Buffer", "Kafka", "Aislar picos")
        Container(proc, "Procesador", "Go", "Enriquecer y normalizar")
        Container(store, "TSDB", "Prometheus", "Métricas")
        Container(traces, "Trace DB", "Tempo", "Trazas distribuidas")
        Container(alerts, "Alerting", "Alertmanager", "Notificaciones")
        Container(ui, "Dashboards", "Grafana", "Visualización")
    }
    Rel(apps, collector, "Exporta", "OTLP/HTTP")
    Rel(collector, queue, "Publica", "Kafka")
    Rel(queue, proc, "Consume", "Kafka")
    Rel(proc, store, "Guarda", "Remote Write")
    Rel(proc, traces, "Guarda", "gRPC")
    Rel(alerts, ui, "Configura", "HTTP")
    Rel(sre, ui, "Consulta", "HTTPS")
```

## Buenas prácticas
- Siempre etiqueta tecnología y responsabilidad para cada contenedor.
- Usa límites (`Container_Boundary`) para separar dominios (tienda, pagos, seguridad).
- Prefiere protocolos explícitos en `Rel` (HTTPS, gRPC, AMQP, SQL).
- Añade caches, colas o buses cuando haya integración asíncrona para reflejar acoplamiento.
- Valida los diagramas en Mermaid Live Editor antes de usarlos en documentación.
