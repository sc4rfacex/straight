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

## Buenas Prácticas
- Claves primarias/foráneas claras.
- Normalización (3NF).
- Asegura integridad referencial.
- Métricas: <10 entidades; relaciones descriptivas 100%.