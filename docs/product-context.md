# Contexto de Producto

## Proposito

Ara World Cup es una app interna para acompanar el Mundial con una dinamica de
predicciones, ranking y gamificacion entre personas y areas de la empresa.

El producto busca que las personas puedan:

- registrar predicciones generales del torneo;
- registrar predicciones diarias por partido;
- seguir su desempeno en ranking individual y por area;
- consultar partidos relevantes del dia;
- ver mensajes y elementos de gamificacion que mantengan vivo el juego.

## Jerarquia de fuentes

Este hub interpreta la reunion con una jerarquia explicita:

- **Alejandra Alvarado** es fuente principal de intencion de producto, UX deseada
  y necesidades no tecnicas.
- **Walter Zetino** es fuente principal de criterio tecnico, arquitectura, riesgos,
  priorizacion y forma de implementacion.

Cuando Alejandra expresa una idea de manera vaga, se documenta como requerimiento
de producto por precisar. Cuando Walter propone un ajuste tecnico, se documenta
como recomendacion tecnica de alto peso.

## Vistas clave

### Home

Requerimiento de producto:

- Mostrar un resumen del dia: partidos, predicciones pendientes y cierre cercano.
- Resaltar si juega una seleccion favorita del usuario.
- Mostrar una vista resumida del ranking general con enlace al ranking completo.
- Mostrar predicciones generales del usuario y partidos de hoy.
- Incluir recordatorios o mensajes relacionados con partidos relevantes, si eso no
  genera confusion operativa.

Guia tecnica:

- Home debe ser complemento, no el centro de la implementacion inicial.
- El foco inicial recomendado es gamificacion, predicciones y ranking.
- Calendario y recordatorios deben quedar como capacidades secundarias porque el
  calendario del Mundial esta disponible por otras vias.

### Predicciones

Requerimiento de producto:

- Separar predicciones generales del torneo y predicciones diarias por partido.
- Permitir que las predicciones generales se completen una vez y luego queden
  bloqueadas.
- Permitir que las predicciones diarias se editen hasta 30 minutos antes del
  inicio de cada partido.
- Explicar dentro de la vista como se ganan puntos.

Guia tecnica:

- El bloqueo diario debe aplicarse por partido, no por jornada completa.
- Una vista diaria puede mostrar varios partidos, pero cada partido debe tener su
  propio estado editable o bloqueado.
- Los partidos cerrados deben mostrarse visualmente como no editables, por ejemplo
  en gris o con estado bloqueado.

### Ranking

Requerimiento de producto:

- Mostrar ranking individual.
- Mostrar ranking por area o equipo interno.
- Destacar un top 3, pero permitir ver el ranking completo.
- Agregar narrativa de gamificacion: rachas, cambios de posicion, aciertos y
  mensajes dinamicos.

Guia tecnica:

- El ranking basico debe existir antes de graficas o visualizaciones avanzadas.
- La agregacion por area requiere definir claramente como se crean y nombran los
  grupos internos.
- Los mensajes de gamificacion deben derivarse de datos confiables, no de frases
  hardcodeadas sin relacion con eventos reales.

## Prioridad funcional sugerida

1. Predicciones y reglas de bloqueo.
2. Scoring y ranking basico.
3. Ranking por area y narrativa de gamificacion.
4. Home como resumen de la experiencia.
5. Calendario, recordatorios y bot de Teams como extensiones.

## Preguntas abiertas principales

- Cual es la tabla final de puntuacion para todas las mecanicas.
- Como se resolveran categorias subjetivas como seleccion sorpresa, seleccion
  decepcion y mejor defensa.
- Si la resolucion con IA/internet sera obligatoria, opcional o solo asistencia
  para administradores.
- Como se definiran areas o equipos internos para ranking grupal.
- Si la foto de perfil vendra desde Microsoft o se cargara manualmente.
