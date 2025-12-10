# Diagramas C4 Container en Mermaid

## Sintaxis Básica (Oficial)
`C4Container` para nivel 2. Elementos: Container(alias, "Label", "Tech").

## Errores Más Comunes
- Contenedores sin tecnología.
- Relaciones sin etiqueta.

## Ejemplos
### Simple (Oficial)
```mermaid
C4Container
    Person(customer, "Customer")
    Container(spa, "SPA", "Angular")
    Rel(customer, spa, "Usa", "HTTPS")
```

### Medio (Del Original)
```mermaid
C4Container
    title Sistema E-commerce - Contenedores
    Person(customer, "Cliente")
    Container_Boundary(c1, "Sistema E-commerce") {
        Container(web, "Aplicación Web", "React", "SPA")
        Container(api, "API Backend", "Node.js", "REST API")
        ContainerDb(db, "Base de Datos", "PostgreSQL", "Almacena productos, pedidos")
        Container(cache, "Cache", "Redis", "Cache de sesiones")
    }
    System_Ext(payment, "Pasarela Pago")
    Rel(customer, web, "Usa", "HTTPS")
    Rel(web, api, "Llamadas API", "JSON/HTTPS")
    Rel(api, db, "Lee/Escribe", "SQL")
    Rel(api, cache, "Cache", "TCP")
    Rel(api, payment, "Procesa pago", "HTTPS")
```

### Complejo (Complemento)
```mermaid
C4Container
    title Contenedor Completo
    Container_Boundary(b, "Boundary")
    {
        Container(c1, "Container1", "Tech1")
        ContainerDb(db, "DB", "SQL")
    }
    Rel(c1, db, "Usa")
```

## Buenas Prácticas
- Incluye tecnología en contenedores.
- Muestra fronteras de sistemas.
- Del original: Enfócate en aplicaciones/servicios.