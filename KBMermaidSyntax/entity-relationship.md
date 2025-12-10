# Diagramas Entidad-Relación (E-R) en Mermaid

## Sintaxis Básica
Inicia con "erDiagram". 
Entidades: [Nombre] { tipo campo }. 
Relaciones: ||--o{ para 1 a muchos, con : "etiqueta".

## Cardinalidad (Oficial + Original)
- `||--||` : Uno a Uno
- `||--o{` : Uno a Muchos
- `}o--o{` : Muchos a Muchos

## Errores Más Comunes
- Relaciones sin etiqueta (: "rol").
- Tipos de datos inválidos (sin string/int).
- Cardinalidad incorrecta.
- Entidades sin campos.
- Del PDF: Relaciones no normalizadas.

## Patrones Anti-Error (❌ vs ✅)
- ❌ `CLIENTE ||-- PEDIDO` (falta cardinalidad completa) → ✅ `CLIENTE ||--o{ PEDIDO : realiza`.
- ❌ Campos sin tipo `string`/`int` → ✅ `string nombre`, `int id PK`.
- ❌ Repetir comillas o caracteres especiales en nombres → ✅ Alias en mayúsculas sin espacios (usa guiones bajos).
- ❌ Relaciones sin dirección semántica → ✅ Etiquetas verbales (`: envía`, `: factura`).
- Checklist: cardinalidades cerradas, tipos explícitos, llaves PK/FK indicadas, relaciones con verbo.

## Ejemplos
### Simple (Oficial)
```mermaid
erDiagram
    CLIENTE
```

### Medio (Del Original + PDF)
```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : realiza
    CLIENTE {
        string nombre
        string email
    }
    PEDIDO {
        int id
        date fecha
    }
```

### Complejo (Del Original)
```mermaid
erDiagram
    USUARIO ||--o{ PEDIDO : "realiza"
    PEDIDO ||--|{ LINEA_PEDIDO : "contiene"
    PRODUCTO ||--o{ LINEA_PEDIDO : "se incluye en"
    CATEGORIA ||--o{ PRODUCTO : "agrupa"
    USUARIO {
        int id PK
        string email UK
        string nombre
    }
    PEDIDO {
        int id PK
        int usuario_id FK
        float total
    }
    PRODUCTO {
        int id PK
        int categoria_id FK
        string nombre
    }
    LINEA_PEDIDO {
        int pedido_id FK
        int producto_id FK
        int cantidad
    }
    CATEGORIA {
        int id PK
        string nombre
    }
```

### Banca - Cuentas y transacciones
```mermaid
erDiagram
    CLIENTE ||--o{ CUENTA : posee
    CUENTA ||--o{ TRANSACCION : registra
    CUENTA ||--o{ TARJETA : emite
    TRANSACCION }o--|| COMERCIO : ocurre_en
    CLIENTE {
        int id PK
        string nombre
        string documento UK
    }
    CUENTA {
        int id PK
        string tipo
        float saldo
        int cliente_id FK
    }
    TARJETA {
        int id PK
        string marca
        date fecha_vencimiento
        int cuenta_id FK
    }
    TRANSACCION {
        int id PK
        datetime fecha
        float monto
        int cuenta_id FK
        int comercio_id FK
    }
    COMERCIO {
        int id PK
        string nombre
        string categoria
    }
```

### Salud - Agenda y órdenes médicas
```mermaid
erDiagram
    PACIENTE ||--o{ CITA : agenda
    MEDICO ||--o{ CITA : atiende
    CITA ||--o{ ORDEN : genera
    ORDEN }o--|| LABORATORIO : se_envia_a
    PACIENTE {
        int id PK
        string nombre
        date fecha_nacimiento
    }
    MEDICO {
        int id PK
        string especialidad
    }
    CITA {
        int id PK
        datetime fecha
        int paciente_id FK
        int medico_id FK
        string estado
    }
    ORDEN {
        int id PK
        int cita_id FK
        string tipo
    }
    LABORATORIO {
        int id PK
        string nombre
        string acreditacion
    }
```

### Supply Chain - Inventario multi almacén
```mermaid
erDiagram
    PRODUCTO ||--o{ STOCK : tiene
    ALMACEN ||--o{ STOCK : almacena
    STOCK }o--o{ LOTE : lotifica
    PRODUCTO {
        int id PK
        string nombre
        string sku UK
    }
    ALMACEN {
        int id PK
        string region
        string tipo
    }
    STOCK {
        int id PK
        int producto_id FK
        int almacen_id FK
        int cantidad
    }
    LOTE {
        int id PK
        int stock_id FK
        date fecha_vencimiento
        string origen
    }
```

## Buenas Prácticas
- Claves primarias/foráneas claras.
- Normalización (3NF).
- Asegura integridad referencial.
- Métricas: <10 entidades; relaciones descriptivas 100%.
- Troubleshooting: verifica que cada relación tenga verbo, cardinalidad completa y que todas las FK estén declaradas en la entidad hija para evitar errores de parseo.