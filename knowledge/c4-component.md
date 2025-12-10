# Diagramas C4 Component en Mermaid

## Sintaxis Básica (Oficial)
`C4Component` para nivel 3. 
Elementos: Component(alias, "Label", "Tech").

## Errores Más Comunes
- Componentes sin contenedor padre.
- Relaciones sin tecnología.

## Ejemplos
### Simple (Oficial)
```mermaid
C4Component
    Container(api, "API")
    Component(ctrl, "Controller", "Express")
    Rel(ctrl, api, "Usa")
```

### Medio (Del Original)
```mermaid
C4Component
    title API Backend - Componentes
    Container_Boundary(api, "API Backend") {
        Component(authController, "Auth Controller", "Express", "Autenticación")
        Component(productController, "Product Controller", "Express", "Productos")
        Component(orderService, "Order Service", "Service", "Lógica de pedidos")
        Component(paymentService, "Payment Service", "Service", "Integración pagos")
        ComponentDb(orm, "ORM", "Sequelize", "Acceso a datos")
    }
    ContainerDb(db, "Database")
    System_Ext(payment, "Pasarela")
    Rel(authController, orm, "Usa")
    Rel(productController, orm, "Usa")
    Rel(orderService, orm, "Usa")
    Rel(orderService, paymentService, "Procesa pago")
    Rel(paymentService, payment, "API Call")
    Rel(orm, db, "SQL")
```

### Complejo (Complemento)
```mermaid
C4Component
    title Componente Completo
    Container_Boundary(b, "Boundary")
    {
        Component(c1, "Comp1", "Tech1")
        ComponentDb(db, "DB Comp", "SQL")
    }
    Rel(c1, db, "Usa")
```

## Buenas Prácticas
- Muestra lógica interna de contenedores.
- Usa SOLID.
- Del original: Enfócate en módulos reutilizables.