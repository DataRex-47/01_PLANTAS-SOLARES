# Hallazgos del análisis

Una fila por pregunta respondida. La columna ⭐ marca la que rompe la hipótesis de partida.

| Id | Pregunta | Respuesta con número | Hipótesis | ⭐ | Trampas revisadas | Gráfico |
|---|---|---|---|---|---|---|
| 2.1 | ¿Llega suficiente irradiación todos los días? | 34 jornadas completas con media de 5,5 kWh/m² al día y pico de 1,22 en la columna de irradiación | llegaba irradiación suficiente | no | unidad de la columna revisada y marcada | captacion_planta.png |
| 2.2 | ¿Es similar la captación en las dos plantas? | Planta 2 recibe 12 % menos irradiación (0,37 frente a 0,42) y tiene 1,49 °C más de ambiente, con el módulo casi igual (31,56 contra 31,80) | Planta 2 captaba menos por ubicación | no | promedio solo con sol, noche excluida | captacion_planta.png |
| 2.3 | ¿Cómo se distribuye la irradiación por hora? | curva en campana entre las 7 y las 18, con pico a las 12 y 13 en las dos plantas | una sola planta tenía sombra a media mañana | no | un inversor por planta, límite declarado | irradiacion_hora.png |
| 3.1 | ¿Cuál es la relación entre irradiación y generación? | relación lineal hasta el codo de la planta, sin saturación temprana en ninguno de los dos parques | la conversión se perdía antes del codo | no | revisado por planta | dc_irradiacion.png |
| 3.2 | ¿La temperatura afecta la generación? | el módulo está a la misma temperatura en las dos plantas, con 0,24 °C de diferencia | Planta 2 perdía por calor | no | temperatura de módulo además de ambiente | dc_irradiacion.png |
| 3.3 | ¿Es constante el comportamiento en todos los inversores? | no: cinco equipos de Planta 2 suman entre 54,25 y 74 horas caídas frente a 5,75 del peor de Planta 1 | todos los inversores se comportaban igual | sí | noche excluida, equipos sin registro no suman | heatmap_dias.png |
| 3.4 | ¿Ha habido momentos de fallo? | sí, 11 días entre el 15 de mayo y el 14 de junio, con el 7 de junio como peor día en las dos plantas | los fallos eran eventos aislados | sí | media horaria complementada con revisión manual del 20 de mayo | heatmap_inversores.png |

Los gráficos viven en `06_resultados/Analisis/graficos/`. El detalle de cada cifra está en `informe_analisis.md`.
