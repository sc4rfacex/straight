
# Diagramas C4 Context en Mermaid

## Sintaxis Básica (Oficial)
`C4Context` para nivel 1. 
Elementos: Person(alias, "Label"), 
System(alias, "Label").

## Errores Más Comunes
- Missing braces for boundaries.
- Relaciones a elementos no definidos.
- Del original: Usar Node() en lugar de Person/System.

## Ejemplos
### Simple
```mermaid
C4Context
    Person(customer, "Customer")
    System(system, "System")
    Rel(customer, system, "Uses")
```

### Medio
```mermaid
C4Context
    title Sistema de E-commerce - Contexto
    Person(customer, "Cliente", "Usuario que compra productos")
    Person(admin, "Administrador", "Gestiona el sistema")
    System(ecommerceSystem, "Sistema E-commerce", "Permite comprar productos online")
    System_Ext(paymentGateway, "Pasarela de Pago", "Procesa pagos")
    System_Ext(emailSystem, "Sistema de Email", "Envía notificaciones")
    Rel(customer, ecommerceSystem, "Compra productos", "HTTPS")
    Rel(admin, ecommerceSystem, "Administra", "HTTPS")
    Rel(ecommerceSystem, paymentGateway, "Procesa pagos", "API REST")
    Rel(ecommerceSystem, emailSystem, "Envía emails", "SMTP")
```

### Complejo
```mermaid
C4Context
    title Contexto Completo
    Enterprise_Boundary(b, "Empresa")
    {
        Person(cust, "Cliente")
        System(ib, "Sistema Interno")
    }
    System_Ext(ext, "Sistema Externo")
    Rel(cust, ib, "Usa")
    Rel(ib, ext, "Integra")
```

## Buenas Prácticas
- Muestra actores externos.
- Usa boundaries para agrupar.
- Del original: Enfócate en "qué" no "cómo".