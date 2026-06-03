# ADR-0010: Proponer ranking grupal normalizado y vista de desempeno personal

## Estado

Proposed

## Contexto

El producto contempla ranking individual y ranking por area o equipo interno. Si
los grupos tienen tamanos diferentes, una suma bruta de puntos puede favorecer a
grupos grandes. Por ejemplo, un grupo de devs con mas de 20 personas podria
superar a un grupo de producto con 10 personas solo por volumen.

Tambien existe una necesidad de explicar como cada usuario gano puntos. Esa
informacion puede crecer lo suficiente como para merecer una vista propia, en vez
de quedar comprimida en una card dentro del ranking global.

## Decision

Se propone que el ranking por grupos no dependa solo de suma bruta cuando los
grupos tengan tamanos distintos.

Opciones de normalizacion a evaluar:

- promedio por participante activo;
- promedio top N por grupo;
- ponderacion por participacion;
- suma bruta visible solo como dato secundario.

Tambien se propone separar Ranking en dos subvistas:

- ranking global, con tabla, top 3, filtros, sort y paginado;
- desempeno personal, con log de puntos, desglose de aciertos y movimientos.

## Alternativas consideradas

### Suma bruta por grupo

- Ventajas: facil de explicar y calcular.
- Desventajas: favorece grupos grandes y puede percibirse injusta.

### Promedio simple

- Ventajas: corrige diferencia de tamano.
- Desventajas: puede favorecer grupos pequenos con pocos participantes activos.

### Top N por grupo

- Ventajas: compara grupos con una muestra equivalente.
- Desventajas: puede excluir contribuciones de participantes fuera del top N.

### Subvista personal separada

- Ventajas: permite explicar puntos sin saturar ranking global.
- Desventajas: agrega navegacion y requiere modelo auditable de eventos de
  scoring.

## Consecuencias

### Positivas

- El ranking grupal puede percibirse mas justo.
- El ranking global puede enfocarse en comparacion y tabla.
- El desempeno personal puede mostrar trazabilidad de puntos con mas claridad.

### Negativas

- La formula de normalizacion debe definirse y explicarse cuidadosamente.
- Se necesita registrar eventos o desglose de scoring de forma auditable.
- Paginado, sort y filtros agregan complejidad de UI y consulta.

## Fuente de la decision/propuesta

- Criterio tecnico del usuario.
- Antecedente de producto: Alejandra solicito capacidades de tabla como paginado,
  sort y filtros para ranking.
- Contexto previo: ranking por area/equipo interno aparece como requerimiento de
  producto.

