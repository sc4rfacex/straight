# Diagramas C4 Context en Mermaid

Nivel 1 del modelo C4. Describe actores y sistemas involucrados, sin detallar componentes internos.

## Sintaxis completa
```mermaid
C4Context
    title [Título opcional]
    Person(alias, "Etiqueta", "Descripción")
    System(alias, "Etiqueta", "Descripción")
    System_Ext(alias, "Etiqueta", "Descripción")
    Enterprise_Boundary(id, "Nombre") { ... }
    Rel(origen, destino, "Relación", "Protocolo")
```

### Errores frecuentes
- Olvidar cerrar llaves de `Enterprise_Boundary`.
- Referenciar aliases no definidos en `Rel`.
- Mezclar mayúsculas/minúsculas creando actores duplicados.
- Abusar de detalles técnicos (puertos, tablas) que pertenecen a niveles inferiores.

## Ejemplos

### Simple - SaaS genérico
```mermaid
C4Context
    title Contexto SaaS
    Person(user, "Usuario")
    System(app, "Aplicación SaaS")
    System_Ext(email, "Correo", "Notificaciones")
    Rel(user, app, "Usa")
    Rel(app, email, "Envía alertas", "SMTP")
```

### Medio - E-commerce con pasarela y email
```mermaid
C4Context
    title Sistema de E-commerce
    Person(customer, "Cliente", "Compra productos")
    Person(admin, "Administrador", "Gestiona catálogo")
    System(shop, "Tienda Online", "Catálogo, carrito y pagos")
    System_Ext(payment, "Pasarela de Pago", "Autoriza transacciones")
    System_Ext(mail, "Email", "Confirmaciones y marketing")
    Rel(customer, shop, "Navega y paga", "HTTPS")
    Rel(admin, shop, "Administra productos", "HTTPS")
    Rel(shop, payment, "Procesa pagos", "API REST")
    Rel(shop, mail, "Envía notificaciones", "SMTP")
```

### Complejo - Banco digital con regulación
```mermaid
C4Context
    title Contexto bancario
    Enterprise_Boundary(bank, "Banco Digital") {
        Person(customer, "Cliente", "Opera cuentas y préstamos")
        Person(ops, "Operaciones", "Atiende excepciones")
        System(core, "Core Bancario", "Cuentas y transacciones")
        System(kyc, "KYC/AML", "Validación regulatoria")
    }
    System_Ext(regulator, "Regulador", "Reportes normativos")
    System_Ext(paynet, "Red de Pagos", "Compensación interbancaria")
    Rel(customer, core, "Consulta y transfiere", "App/Web")
    Rel(customer, kyc, "Envía documentos", "HTTPS")
    Rel(ops, core, "Gestiona casos", "Intranet")
    Rel(core, kyc, "Valida riesgos", "Batch/API")
    Rel(core, regulator, "Reporta operaciones", "SFTP")
    Rel(core, paynet, "Liquida pagos", "ISO 20022")
```

### Complejo - Salud (telemedicina)
```mermaid
C4Context
    title Plataforma de Telemedicina
    Enterprise_Boundary(clinic, "Clínica") {
        Person(patient, "Paciente", "Solicita consulta")
        Person(doctor, "Médico", "Atiende en línea")
        System(emr, "Historia Clínica", "Registra consultas")
        System(app, "App de Citas", "Agenda y video")
    }
    System_Ext(pharmacy, "Farmacia", "Dispensa medicación")
    System_Ext(lab, "Laboratorio", "Resultados de estudios")
    Rel(patient, app, "Agenda y paga", "Móvil/Web")
    Rel(doctor, app, "Realiza videollamada", "WebRTC")
    Rel(app, emr, "Guarda consulta", "API")
    Rel(app, pharmacy, "Envía receta", "API")
    Rel(emr, lab, "Solicita estudios", "HL7/FHIR")
```

### Complejo - Gobierno electrónico
```mermaid
C4Context
    title Portal de trámites
    Enterprise_Boundary(gov, "Gobierno") {
        Person(citizen, "Ciudadano", "Solicita servicios")
        Person(agent, "Agente", "Valida expedientes")
        System(portal, "Portal Único", "Inicio y seguimiento")
        System(backoffice, "Backoffice", "Gestión interna")
    }
    System_Ext(idp, "Identidad Digital", "Autenticación")
    System_Ext(payments, "Pagos", "Tasas y multas")
    Rel(citizen, portal, "Inicia trámite", "Web")
    Rel(portal, idp, "Autentica", "OIDC")
    Rel(portal, payments, "Cobra tasas", "API")
    Rel(portal, backoffice, "Envía expediente", "Message Bus")
    Rel(agent, backoffice, "Revisa y aprueba", "Intranet")
```

### Complejo - Retail omnicanal
```mermaid
C4Context
    title Retail omnicanal
    Enterprise_Boundary(retail, "Retail") {
        Person(shopper, "Comprador", "Compra en web y tienda")
        System(web, "E-commerce", "Catálogo y checkout")
        System(pos, "POS Tienda", "Cobro presencial")
        System(cdm, "CDM", "Gestión de clientes y loyalty")
    }
    System_Ext(wms, "WMS", "Inventario y surtido")
    System_Ext(marketing, "CDP", "Campañas personalizadas")
    Rel(shopper, web, "Compra online", "HTTPS")
    Rel(shopper, pos, "Compra presencial", "NFC/EMV")
    Rel(web, wms, "Reserva inventario", "API")
    Rel(pos, wms, "Sincroniza stock", "Batch")
    Rel(web, cdm, "Actualiza perfil", "API")
    Rel(cdm, marketing, "Segmenta", "ETL")
```

### Experto - Plataforma de datos (streaming)
```mermaid
C4Context
    title Plataforma de datos en tiempo real
    Enterprise_Boundary(data, "Empresa de Datos") {
        Person(analyst, "Analista", "Crea dashboards")
        System(stream, "Ingesta Streaming", "Kafka/Pulsar")
        System(warehouse, "Data Warehouse", "BI y modelos")
        System(catalog, "Catálogo", "Gobernanza")
    }
    System_Ext(sources, "Sistemas Fuente", "Apps, IoT")
    System_Ext(bi, "BI", "Consumo analítico")
    Rel(sources, stream, "Publica eventos", "Producers")
    Rel(stream, warehouse, "Carga incremental", "ETL/ELT")
    Rel(warehouse, bi, "Consulta datasets", "SQL/ODBC")
    Rel(warehouse, catalog, "Registra esquemas", "API")
    Rel(analyst, bi, "Explora métricas", "Dashboards")
```

### Experto - Seguridad y monitoreo
```mermaid
C4Context
    title Seguridad y monitoreo cloud
    Enterprise_Boundary(sec, "Empresa") {
        Person(analyst, "SOC Analyst", "Responde alertas")
        System(siem, "SIEM", "Correlación y alertas")
        System(soar, "SOAR", "Automatiza respuestas")
        System(assets, "CMDB", "Inventario de activos")
    }
    System_Ext(cloud, "Proveedores Cloud", "Logs y eventos")
    System_Ext(tickets, "Helpdesk", "Incidentes")
    Rel(cloud, siem, "Envía logs", "Syslog/API")
    Rel(siem, soar, "Dispara playbooks", "Webhook")
    Rel(soar, tickets, "Crea tickets", "API")
    Rel(soar, assets, "Actualiza estado", "API")
    Rel(analyst, siem, "Investiga alertas", "UI")
```

### Experto - Fintech pagos internacionales
```mermaid
C4Context
    title Pagos transfronterizos
    Enterprise_Boundary(fintech, "Fintech") {
        Person(customer, "Cliente", "Envía y recibe pagos")
        System(app, "App Fintech", "Wallet y transferencias")
        System(risk, "Motor de Riesgo", "Fraude y AML")
    }
    System_Ext(banks, "Bancos corresponsales", "Liquidación")
    System_Ext(kyc, "Proveedor KYC", "Verificación identidad")
    System_Ext(fx, "FX", "Tipos de cambio")
    Rel(customer, app, "Transfiere fondos", "Móvil/Web")
    Rel(app, risk, "Evalúa operación", "API interna")
    Rel(risk, kyc, "Valida identidad", "API")
    Rel(app, banks, "Envía orden", "SWIFT/ISO20022")
    Rel(app, fx, "Consulta tipo de cambio", "API")
```

## Buenas prácticas
- Usa títulos y descripciones breves orientadas al usuario y objetivo del sistema.
- Aísla sistemas externos con `System_Ext` para clarificar dependencias y responsabilidades.
- Usa `Enterprise_Boundary` para mostrar dominios/regiones (negocio vs terceros).
- Mantén protocolos en las relaciones solo si aportan claridad (HTTPS, API, Batch).
