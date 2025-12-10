# Diagramas de User Journey

Los diagramas de `journey` en Mermaid permiten mapear experiencias de usuario con métricas de satisfacción paso a paso. Usa sangrías con espacios (no tabs) y mantén puntuaciones de 1 a 5.

## Sintaxis completa
```mermaid
journey
    title [Título]
    section [Nombre de la sección]
        Actor: Puntuación: Acción detallada
        // Puntuación: 1-5 (experiencia negativa a muy positiva)
```

### Errores comunes a evitar
- Omitir `journey` como palabra clave inicial.
- Usar tabs en lugar de espacios (rompe el render).
- Puntuaciones fuera de 1-5 o sin número.
- Falta de secciones: todos los pasos deben estar anidados en `section`.
- Repetir actores con mayúsculas/minúsculas distintas generando dos entidades.

## Ejemplos progresivos
Cada ejemplo sigue la plantilla recomendada: título claro, secciones con contexto y puntuaciones realistas.

### Nivel: Simple - Login web
**Descripción**: Flujo mínimo de autenticación.
**Complejidad**: Simple
**Elementos clave**: credenciales válidas, recuperación de cuenta.
```mermaid
journey
    title Inicio de sesión básico
    section Acceso
        Usuario: 2: Ingresa credenciales incorrectas
        Usuario: 4: Reintenta con credenciales correctas
        Usuario: 5: Accede al panel principal
```
**Errores comunes evitados**: secciones con 3+ pasos coherentes; puntuación mejora tras resolver error.

### Nivel: Medio - Registro SaaS con doble factor
**Descripción**: Alta de cuenta y verificación MFA.
**Complejidad**: Medio
**Elementos clave**: MFA, correo transaccional, fallback SMS.
```mermaid
journey
    title Registro con MFA
    section Alta
        Prospecto: 4: Completa formulario de registro
        Prospecto: 3: Confirma email de activación
    section Seguridad
        Prospecto: 2: Fallo al configurar app de autenticación
        Prospecto: 4: Usa SMS como método alterno
        Prospecto: 5: Accede con MFA habilitado
```
**Errores comunes evitados**: separa onboarding funcional de seguridad; muestra camino de recuperación.

### Nivel: Medio - Compra e-commerce web
**Descripción**: Journey completo desde búsqueda a pago.
**Complejidad**: Medio
**Elementos clave**: filtros, carrito, gateway pago.
```mermaid
journey
    title Compra en tienda online
    section Descubrimiento
        Cliente: 4: Busca producto con filtros
        Cliente: 3: Revisa reseñas y compara variantes
    section Conversión
        Cliente: 5: Añade al carrito y aplica cupón
        Cliente: 4: Completa pago con tarjeta
    section Postventa
        Cliente: 5: Recibe confirmación y seguimiento
```
**Errores comunes evitados**: balance de secciones; incluye etapa de postventa para métricas NPS.

### Nivel: Medio - Soporte en mesa de ayuda
**Descripción**: Ticketing con escalamiento.
**Complejidad**: Medio
**Elementos clave**: SLA, comunicación proactiva.
```mermaid
journey
    title Soporte técnico
    section Reporte
        Usuario: 2: Envía ticket con error crítico
        Agente: 3: Solicita logs y evidencia
    section Resolución
        Agente: 4: Escala a nivel 2
        Ingeniero: 5: Aplica fix y valida
    section Cierre
        Usuario: 4: Recibe comunicación de resolución
```
**Errores comunes evitados**: actores distintos por rol; cada sección con responsabilidad clara.

### Nivel: Complejo - Onboarding multi-actor (Banca)
**Descripción**: KYC y activación de cuenta.
**Complejidad**: Complejo
**Elementos clave**: verificación identidad, revisión manual, activación multicanal.
```mermaid
journey
    title Onboarding bancario digital
    section Registro
        Solicitante: 3: Captura datos personales y selfies
        Sistema: 4: Valida documentos con OCR
    section Revisión
        Analista: 2: Detecta coincidencia dudosa en listas de riesgo
        Analista: 4: Solicita información adicional
        Solicitante: 4: Sube comprobante complementario
    section Activación
        Sistema: 5: Autoriza cuenta y habilita tarjeta virtual
        Solicitante: 5: Recibe PIN temporal vía app
```
**Errores comunes evitados**: mezcla de actores humanos y sistemas; decisiones de riesgo reflejadas en puntuación.

### Nivel: Complejo - Onboarding SaaS B2B
**Descripción**: Setup con data import y capacitación.
**Complejidad**: Complejo
**Elementos clave**: migración CSV, roles admin/usuario, entrenamiento.
```mermaid
journey
    title Onboarding SaaS B2B
    section Configuración
        AdminCliente: 3: Carga CSV con errores de formato
        Sistema: 4: Muestra validación y guía correcciones
        AdminCliente: 5: Importa datos limpios
    section Habilitación
        AdminCliente: 4: Asigna roles y permisos
        Equipo: 3: Completa tutorial interactivo
        Equipo: 5: Utiliza panel con dashboards activos
```
**Errores comunes evitados**: destaca corrección de datos; rol administrativo separado del equipo.

### Nivel: Complejo - Banca móvil con recobro
**Descripción**: Flujo de pago recurrente y recuperación.
**Complejidad**: Complejo
**Elementos clave**: pago automático, error, reintento seguro.
```mermaid
journey
    title Pago recurrente móvil
    section Cobro
        Cliente: 4: Programa pago automático
        Sistema: 2: Falla cobro por fondos insuficientes
    section Recuperación
        Cliente: 3: Actualiza método de pago
        Sistema: 5: Reintenta y confirma pago
    section Notificación
        Cliente: 5: Recibe recibo digital y recordatorio
```
**Errores comunes evitados**: incluye camino de error y recuperación explícitos.

### Nivel: Experto - Healthcare telemedicina
**Descripción**: Consulta remota con seguimiento clínico.
**Complejidad**: Experto
**Elementos clave**: triage, receta electrónica, monitoreo continuo.
```mermaid
journey
    title Consulta telemedicina
    section Pre-consulta
        Paciente: 3: Completa cuestionario de síntomas
        Sistema: 4: Prioriza cita según triage
    section Consulta
        Médico: 5: Realiza videollamada y confirma diagnóstico
        Sistema: 4: Emite receta electrónica
    section Seguimiento
        Paciente: 4: Recibe recordatorio de medicación
        Sistema: 5: Monitorea signos con wearable
```
**Errores comunes evitados**: diferencia entre acciones clínicas y automáticas; puntuaciones alineadas a bienestar.

### Nivel: Experto - Logística con multicanal
**Descripción**: Última milla y atención al cliente omnicanal.
**Complejidad**: Experto
**Elementos clave**: tracking en tiempo real, chat, reintento de entrega.
```mermaid
journey
    title Entrega última milla
    section Despacho
        Operador: 4: Despacha paquete y asigna ruta
        Cliente: 3: Recibe link de seguimiento
    section Ruta
        Repartidor: 2: Encuentra bloqueo vial
        Repartidor: 4: Reajusta ruta con Waze
    section Entrega
        Cliente: 5: Recibe paquete en segunda visita
        Cliente: 4: Califica atención en chat
```
**Errores comunes evitados**: muestra variación de satisfacción por eventos externos; segundo intento visible.

### Nivel: Experto - SaaS con emociones y métricas
**Descripción**: Journey multi-canal con NPS.
**Complejidad**: Experto
**Elementos clave**: web, móvil, soporte; puntuaciones reflejan emoción.
```mermaid
journey
    title Experiencia multicanal SaaS
    section Web
        Usuario: 3: Aprende funcionalidad con guía interactiva
        Usuario: 4: Configura integraciones vía API
    section Móvil
        Usuario: 5: Recibe notificaciones en tiempo real
        Usuario: 2: Encuentra bug en modo offline
    section Soporte
        Agente: 4: Atiende por chat y entrega workaround
        Usuario: 5: Envía NPS positivo tras corrección
```
**Errores comunes evitados**: relaciona puntuaciones con emociones; muestra impacto de soporte en NPS.

### Nivel: Experto - Recuperación ante incidentes (SRE)
**Descripción**: Flujo de incident response con postmortem.
**Complejidad**: Experto
**Elementos clave**: severidad, canales de comunicación, aprendizaje.
```mermaid
journey
    title Respuesta a incidente crítico
    section Detección
        Monitor: 2: Dispara alerta de latencia alta
        SRE: 3: Declara incidente SEV1 y convoca bridge
    section Mitigación
        SRE: 4: Aplica rollback automatizado
        Cliente: 3: Recibe aviso de impacto y ETA
    section Recuperación
        SRE: 5: Restaura tráfico estable
        Equipo: 4: Documenta postmortem y acciones preventivas
```
**Errores comunes evitados**: enfatiza comunicación con clientes; cierra con aprendizaje.

## Checklist final antes de compartir
- Título y secciones describen claramente el contexto.
- Cada paso incluye Actor + Puntuación (1-5) + Acción.
- Secciones no vacías y con mínimo dos pasos.
- Puntuaciones reflejan emoción realista (errores con 1-2, éxitos con 4-5).
- Los actores mantienen el mismo casing en todo el diagrama.
