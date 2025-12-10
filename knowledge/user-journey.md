# Diagramas de User Journey

Sintaxis Básica:
journey
    title [Título]
    section [Sección]
        Actor: Puntuación: Tarea
        // Puntuación: 1-5 (satisfacción)

## Ejemplo (del PDF):
```mermaid

journey
    title Proceso de Compra
    section Búsqueda
      ElUsuario: 3: Abre la aplicación y busca productos
      ElUsuario: 4: Filtra resultados por categoría
    section Compra
      ElUsuario: 5: Agrega un producto al carrito
      ElUsuario: 5: Realiza el pago y completa la compra
```
## Ejemplo Avanzado: Con Múltiples Actores

```mermaid
journey
    title Onboarding
    section Registro
      NuevoUsuario: 4: Crea cuenta
      Admin: 2: Verifica email
```
Checklists:
- Desarrollo: Incluye touchpoints UI/UX.
- Integración: Conecta con APIs externas.
- Métricas: Puntuación media >3; pasos <10 para usabilidad.