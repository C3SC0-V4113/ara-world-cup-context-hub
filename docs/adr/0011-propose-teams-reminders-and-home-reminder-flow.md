# ADR-0011: Redirigir predicciones desde Home y dejar notificaciones fuera de scope

## Estado

Accepted

## Contexto

Home puede mostrar partidos del dia, selecciones favoritas, ranking resumido y
predicciones pendientes. La duda abierta era si Home debia permitir guardar
predicciones como quick action, redirigir a Predicciones o disparar recordatorios
por Teams.

Tambien existian propuestas sobre bot de Teams, mensajes o recordatorios ligados
a partidos y predicciones pendientes. Para mantener el alcance simple, se decide
dejar notificaciones fuera de scope por ahora.

## Decision

Home no guardara predicciones diarias como quick action.

Las acciones relacionadas con predicciones deben redirigir a la vista dedicada
**Predicciones**, especificamente al tab correspondiente:

- `Predicciones del dia` para acciones sobre partidos actuales.
- `Predicciones generales` para setup o revision de predicciones de torneo.

Las notificaciones, recordatorios y mensajes por Teams quedan fuera de scope. Si
existen botones, items o componentes que quieran notificar, deben quedar
deshabilitados o documentados como no disponibles.

Teams y Microsoft Graph se mantienen dentro del contexto de identidad/perfil,
pero no como canal activo de notificaciones en esta version.

## Alternativas consideradas

### Quick actions en Home para guardar predicciones

- Ventajas: reduce clicks.
- Desventajas: duplica logica de prediccion y puede ocultar reglas, bloqueo o
  validaciones importantes.

### Redireccion desde Home hacia Predicciones

- Ventajas: mantiene una unica experiencia para guardar predicciones.
- Desventajas: requiere que la vista Predicciones sea rapida y bien enfocada.

### Recordatorios por Teams

- Ventajas: aprovecha el canal corporativo natural.
- Desventajas: requiere permisos, configuracion, manejo de errores y control de
  ruido. Queda fuera de scope por ahora.

## Consecuencias

### Positivas

- Home se mantiene como resumen y navegacion.
- Se evita duplicar reglas de bloqueo, validacion y guardado.
- La integracion Microsoft se concentra en identidad, perfil y avatar.
- Se reduce alcance inicial al deshabilitar notificaciones.

### Negativas

- Usuarios deben entrar a Predicciones para guardar o editar datos.
- Recordatorios no ayudaran a reducir omisiones en esta fase.
- Componentes de notificacion existentes deben comunicar estado deshabilitado o
  no disponible.

## Fuente de la decision

- Decision posterior del usuario: Home redirige a Predicciones, no guarda quick
  actions, y notificaciones quedan fuera de scope/deshabilitadas.
- Contexto previo: ADR-0007 acepta identidad corporativa Microsoft.
- Contexto previo: ADR-0005 prioriza gamificacion y ranking antes de
  recordatorios.
