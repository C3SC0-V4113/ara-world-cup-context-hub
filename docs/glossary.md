# Glosario

## Prediccion general

Prediccion de largo plazo para todo el torneo. Incluye categorias como campeon,
finalista, seleccion sorpresa, seleccion decepcion, seleccion con mas goles y
mejor defensa.

Fuente de producto: Alejandra. Guia tecnica: debe guardarse por usuario y quedar
bloqueada cuando cierre la etapa general.

## Prediccion diaria

Prediccion asociada a un partido especifico del dia. Incluye ganador, empate,
marcador y si ambos equipos anotan.

Fuente de producto: Alejandra. Guia tecnica: Walter recomienda que el bloqueo sea
por partido individual.

## Seleccion sorpresa

Equipo que llega mas lejos o rinde mejor de lo esperado. Es una categoria
subjetiva.

Guia tecnica: debe resolverse con criterio administrable, posiblemente asistido
por IA/internet, pero con correccion manual.

## Seleccion decepcion

Equipo favorito o esperado que queda eliminado antes o rinde peor de lo previsto.
Es una categoria subjetiva.

Guia tecnica: debe tratarse igual que seleccion sorpresa, con resolucion
administrable.

## Mejor defensa

Seleccion que recibe menos goles o muestra mejor desempeno defensivo, segun el
criterio final que se defina.

Pregunta abierta: si se evaluara estrictamente por goles recibidos o por criterio
asistido.

## Seleccion con mas goles

Seleccion que acumula mas goles durante el torneo. Esta categoria es mas objetiva
que sorpresa o decepcion.

Pregunta abierta: criterio de desempate si dos o mas selecciones terminan con la
misma cantidad de goles.

## Partido con mas goles del dia

Prediccion diaria adicional donde el usuario elige cual partido tendra mayor suma
de goles.

Pregunta abierta: como asignar puntos si varios partidos empatan en total de
goles.

## Bloqueo por partido

Regla que impide editar una prediccion cuando faltan 30 minutos o menos para el
inicio de ese partido.

Fuente tecnica fuerte: Walter. Esta regla evita bloquear partidos posteriores de
la misma jornada.

## Ranking individual

Tabla de participantes ordenada por puntos acumulados. Debe permitir ver el
ranking completo y no solo el top 3.

## Ranking por area

Agregacion de puntos por area, equipo interno o grupo configurable.

Pregunta abierta: si las areas vendran de Microsoft, de un admin interno o de una
configuracion propia del juego.

## Gamificacion

Elementos que hacen mas viva la competencia: top 3, cambios de posicion, rachas,
mensajes tipo Kahoot, badges y movimientos recientes.

Guia tecnica: priorizar narrativa basada en datos reales y ranking confiable.

## Teams bot

Canal potencial para recordatorios, mensajes o consultas rapidas. Se prefiere como
complemento antes que bloquear calendarios corporativos.

## Identidad corporativa

Cuenta laboral usada por las personas de la empresa en el ecosistema Microsoft,
incluyendo Windows y Teams. Es la identidad objetivo para ingresar a Ara World
Cup.

## Microsoft Entra ID

Proveedor corporativo esperado para autenticar usuarios. Puede entregar un id
estable, nombre, correo y fotografia si los permisos del tenant lo permiten.

## Usuario normal

Rol `user`. Participante del juego que puede ver la app, gestionar sus
predicciones y participar en ranking.

## Admin

Rol o panel responsable de operar reglas, resultados, cierres, categorias
subjetivas y recalculos de ranking.

## Administrador

Rol `admin`. Usuario con permisos extra para operar configuracion, cierres,
resultados, scoring y resoluciones subjetivas. Sus permisos deben validarse en
servidor o servicios, no solo en la UI.

## Avatar corporativo

Fotografia de perfil obtenida desde Microsoft cuando esta disponible. Si no se
recibe, la app debe usar iniciales u otro fallback visual.

## Fuente canonica

Lugar donde la app considera que vive el dato valido para operar UI, scoring y
ranking. Para Ara World Cup, la fuente canonica interna es Cloudflare D1.

## API externa

Proveedor o servicio fuera de la app que entrega datos estructurados, por ejemplo
fixtures, resultados, eventos, standings, estadisticas o perfil corporativo.

## Ingestion

Proceso que trae datos desde una API externa o dataset, los normaliza y los guarda
o cachea en D1 antes de que la app los use.

## Dato verificable

Dato objetivo que puede comprobarse con una fuente estructurada o oficial, como
horario de partido, marcador final, goles, tarjetas, standings o top scorers.

## Dato ambiguo

Dato que requiere interpretacion o criterio de negocio, como seleccion sorpresa o
seleccion decepcion. Puede recibir asistencia de IA, pero la resolucion final debe
quedar validada por admin.

## Override admin

Correccion manual hecha por un administrador sobre un dato importado o calculado.
Debe ganar sobre la API externa y conservar suficiente contexto para auditoria.

## Normalizacion de ranking por grupo

Propuesta para comparar areas o equipos internos con tamanos distintos sin usar
solo suma bruta de puntos. Puede evaluarse con promedio por participante activo,
top N o ponderacion por participacion.

Estado: propuesta no aprobada por ADR-0010.

## Log de puntos

Registro que explica como un usuario gano puntos: prediccion, resultado,
criterio de scoring, puntos obtenidos y momento de calculo.

Estado: propuesta no aprobada por ADR-0010.

## Subvista personal

Vista o seccion enfocada en el desempeno de un usuario, separada del ranking
global. Podria contener log de puntos, rachas, aciertos y movimientos.

Estado: propuesta no aprobada por ADR-0010.

## Recordatorio Teams

Mensaje o aviso enviado mediante Teams para recordar partidos, cierres o
predicciones pendientes.

Estado: propuesta no aprobada por ADR-0011.

## Badge de seleccion favorita

Indicador visual para una seleccion favorita del usuario. Esta abierta la
decision entre un estilo estandar para todos los paises o colores propios por
seleccion.

Estado: pregunta abierta en propuestas por vista.

## Sync run

Ejecucion de un proceso de ingestion que trae datos desde un proveedor externo,
registra inicio, fin, estado, errores y volumen de datos procesado.

Estado: propuesta no aprobada por ADR-0012.

## Provider entity

Mapeo entre un identificador externo de proveedor y una entidad interna de la app,
por ejemplo equipo, partido, jugador o competencia.

Estado: propuesta no aprobada por ADR-0012.

## Raw provider payload

Respuesta cruda de un proveedor externo guardada como JSON para auditoria,
debugging o reconciliacion.

Estado: propuesta no aprobada por ADR-0012.

## Score event

Registro auditable de puntos ganados por un usuario, asociado a una regla,
prediccion, partido o categoria.

Estado: propuesta no aprobada por ADR-0012.

## Ranking snapshot

Foto calculada del ranking en un momento especifico. Permite evitar recalcular
todo en cada lectura y facilita comparar cambios de posicion.

Estado: propuesta no aprobada por ADR-0012.
