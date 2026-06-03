# ADR-0011: Proponer recordatorios por Teams y flujo desde Home

## Estado

Proposed

## Contexto

La app vive dentro de un ecosistema corporativo basado en Microsoft/Windows/Teams.
Ya existe una decision aceptada de usar identidad corporativa Microsoft. Tambien
hay propuestas y conversaciones sobre recordatorios, bot de Teams y mensajes
ligados a partidos o predicciones pendientes.

El Home puede mostrar partidos del dia, selecciones favoritas y acciones de
recordar. La duda abierta es si esas acciones deben guardar predicciones,
redirigir a Predicciones o disparar mensajes por Teams.

## Decision

Se propone que Home no guarde predicciones diarias como quick action. Las
acciones de prediccion del dia deberian redirigir a la vista Predicciones para
completar el flujo.

Se propone que la accion "recordar" pueda conectarse a un flujo de Teams, por bot
o Microsoft Graph, para enviar avisos utiles a un grupo, canal o usuario segun se
defina.

Teams deberia usarse para mensajes que la gente realmente leeria, no como log
completo de eventos del juego.

## Alternativas consideradas

### Quick actions en Home para guardar predicciones

- Ventajas: reduce clicks.
- Desventajas: duplica logica de prediccion y puede ocultar reglas, bloqueo o
  validaciones importantes.

### Redireccion desde Home hacia Predicciones

- Ventajas: mantiene una unica experiencia para guardar predicciones.
- Desventajas: requiere que la vista Predicciones sea rapida y bien enfocada.

### Recordatorios solo dentro de la app

- Ventajas: menor complejidad de integracion.
- Desventajas: menos visible para usuarios que trabajan en Teams.

### Recordatorios por Teams

- Ventajas: aprovecha el canal corporativo natural.
- Desventajas: requiere permisos, control de ruido y definicion de destinatarios.

## Consecuencias

### Positivas

- Home se mantiene como resumen y navegacion, no como segunda vista de edicion.
- Teams puede mejorar recordatorios sin convertir el calendario en prioridad.
- La accion de recordar puede conectarse con el contexto corporativo real.

### Negativas

- Se debe definir si el mensaje va a usuario, grupo o canal.
- Microsoft Graph o bot requieren permisos, configuracion y manejo de errores.
- Mensajes excesivos pueden generar ruido y rechazo.

## Fuente de la decision/propuesta

- Criterio tecnico del usuario.
- Contexto previo: ADR-0007 acepta identidad corporativa Microsoft.
- Contexto previo: ADR-0005 deja calendario y recordatorios como complemento
  frente a ranking y gamificacion.

