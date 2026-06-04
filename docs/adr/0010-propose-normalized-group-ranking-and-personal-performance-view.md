# ADR-0010: Usar ranking individual y rankings grupales normalizados

## Estado

Accepted

## Contexto

El producto contempla ranking individual y ranking por area o equipo interno. Si
los grupos tienen tamanos diferentes, una suma bruta de puntos favorece a grupos
grandes. Al mismo tiempo, el equipo quiere conservar una vista sin normalizacion
porque muestra volumen total de participacion y puntos acumulados.

Tambien se decidio eliminar rankings por prediccion semanal, prediccion general,
preccion general y prediccion del dia. El ranking se centrara en individual y
grupos.

## Decision

El ranking tendra tres formas principales:

- **Ranking individual**.
- **Ranking por grupos normalizado** por promedio de participantes activos.
- **Ranking por grupos sin normalizacion** por suma bruta de puntos.

La tabla de ranking debe incluir filtros y sort. El top 3 puede seguir existiendo
como elemento de gamificacion, pero no reemplaza la tabla completa.

La normalizacion aprobada para grupos es:

```text
puntos_normalizados = puntos_totales_del_grupo / participantes_activos_del_grupo
```

Un participante activo es un usuario del grupo que ha enviado al menos una
prediccion valida o ha recibido al menos un `score_event` durante el torneo.

La vista de desempeno personal debe existir como vista o subvista enfocada en log
de puntos, desglose de aciertos y movimientos del usuario.

## Alternativas consideradas

### Suma bruta por grupo

- Ventajas: facil de explicar y calcular.
- Desventajas: favorece grupos grandes y puede percibirse injusta.

### Promedio por participante activo

- Ventajas: corrige diferencia de tamano y mantiene una formula entendible.
- Desventajas: puede favorecer grupos pequenos si tienen pocos participantes muy
  activos.

### Top N por grupo

- Ventajas: compara grupos con una muestra equivalente.
- Desventajas: excluye contribuciones de participantes fuera del top N.

### Ponderacion por participacion

- Ventajas: puede balancear justicia y volumen.
- Desventajas: es mas dificil de explicar.

## Consecuencias

### Positivas

- El ranking grupal puede percibirse mas justo.
- La suma bruta sigue disponible para mostrar volumen total.
- La tabla de ranking se centra en comparacion, filtros y sort.
- El desempeno personal permite explicar puntos sin saturar ranking global.

### Negativas

- El sistema debe calcular y guardar participantes activos por grupo.
- Grupos muy pequenos pueden destacar por promedio; la vista sin normalizacion
  ayuda a contextualizarlo.
- Filtros, sort y snapshots agregan complejidad de UI y consulta.

## Fuente de la decision

- Decision posterior del usuario: usar normalizacion por promedio activo,
  conservar ranking grupal sin normalizacion, eliminar rankings por tipo de
  prediccion y enfocar ranking en individual/grupos.
- Antecedente de producto: Alejandra solicito capacidades de tabla como paginado,
  sort y filtros para ranking.
- Contexto previo: ranking por area/equipo interno aparece como requerimiento de
  producto.
