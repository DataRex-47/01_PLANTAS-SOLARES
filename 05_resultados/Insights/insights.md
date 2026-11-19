# Insights

**Resultado:** cuatro insights sobre la disponibilidad de los inversores, ordenados por confianza y después por impacto. Los cuatro caen en una sola palanca, la eficiencia y el estado de los inversores.

| Id | Insight | Evidencia | Confianza | Impacto base | Dueño |
|---|---|---|---|---|---|
| I1 | Planta 2 dejó de generar unos 785 MWh en 34 días por inversores parados con sol, el 16 % de lo que habría generado | DESCRIPTIVO | alta | 589 MWh recuperables, ESTIMADO | Operación y mantenimiento |
| I2 | El problema de Planta 2 es crónico: 30 de 34 días superan 2 inversor-horas caídas, Planta 1 solo 2 | DESCRIPTIVO | alta | misma base de I1 | Operación y mantenimiento |
| I3 | El 95 % de los tramos en cero de Planta 2 cae entre las 9 y las 15 horas, cuando más produce el parque | DESCRIPTIVO | alta | misma base de I1 | Operación y mantenimiento |
| I4 | Cinco inversores de Planta 2 explican el 40 % de la energía perdida estimada | DESCRIPTIVO | alta | 158 a 315 MWh, ESTIMADO | Mantenimiento |
| I5 | Cuatro inversores de Planta 2 no reportan del 19 al 29 de mayo, el 28 % de sus registros | DESCRIPTIVO | alta | hasta 297 MWh más, cota superior no incluida | Operación |

## Fichas

### I1 · Planta 2 dejó de generar unos 785 MWh en 34 días por inversores parados con sol
Palanca: eficiencia y estado de los inversores
Evidencia: DESCRIPTIVO
Soporte: informe de análisis (parones por inversor), gráfico `Exhibits/EX1_inversor_horas_por_dia.png`
Solidez: segmento sí · tiempo sí · atípicos sí · n = 3.436 tramos caídos en Planta 2 y 82 en Planta 1
Impacto: 392 a 785 MWh (base 589), ESTIMADO. Base afectada: 784.877 kWh estimados con la mediana de los inversores vecinos en el mismo instante. Variación supuesta: recuperar entre la mitad y la totalidad. Sin tarifa en los datos, no se convierte a dinero.
Decisión: enviar un equipo de ingenieros a Planta 2 y no a Planta 1 · Dueño: Operación y mantenimiento
Contraintuitivo: se esperaba una pérdida repartida entre las dos plantas. Planta 1 pierde el 0,3 % con el mismo cálculo.

### I2 · El problema de Planta 2 es crónico y no depende de unos pocos días malos
Palanca: eficiencia y estado de los inversores
Evidencia: DESCRIPTIVO
Soporte: informe de análisis (parones por inversor), gráfico `Exhibits/EX1_inversor_horas_por_dia.png`
Solidez: segmento sí · tiempo sí · atípicos sí · n = 34 jornadas
Impacto: la base de I1. Sin los cinco peores días Planta 2 conserva 631,5 inversor-horas caídas en 29 días, frente a 3,75 de Planta 1 en los mismos días.
Decisión: tratar Planta 2 como una condición permanente y no como una lista de incidentes · Dueño: Operación y mantenimiento
Contraintuitivo: el análisis de detalle se hizo sobre cinco días y esos días suman solo el 26 % de las horas caídas. El 74 % queda en los otros 29.

### I3 · El 95 % de los tramos en cero de Planta 2 cae entre las 9 y las 15 horas
Palanca: eficiencia y estado de los inversores
Evidencia: DESCRIPTIVO
Soporte: gráfico `Exhibits/EX2_heatmap_7_junio.png`
Solidez: segmento sí · tiempo sí · atípicos sí · n = 3.250 de 3.436 tramos
Impacto: cada hora perdida al mediodía cuesta más que una de la tarde, porque la planta produce más.
Decisión: pedir al equipo de operación los registros de eventos de los inversores entre las 9 y las 15 horas · Dueño: Operación
Contraintuitivo: el 7 de junio hasta 10 de 22 inversores de Planta 2 estuvieron en cero a la vez. Los datos no dicen por qué, solo cuándo.

### I4 · Cinco inversores de Planta 2 explican el 40 % de la energía perdida estimada
Palanca: eficiencia y estado de los inversores
Evidencia: DESCRIPTIVO
Soporte: informe de análisis (cinco inversores con más energía perdida), gráfico `Exhibits/EX3_top_inversores.png`
Solidez: segmento sí · tiempo sí · atípicos sí · n = 1.323 tramos caídos de esos cinco equipos
Impacto: 158 a 315 MWh (base 236), ESTIMADO. Base: 315.063 kWh que suman los cinco. Variación supuesta: recuperar entre la mitad y la totalidad.
Decisión: empezar la visita por Et9k, LYwn, Quc1, rrq4 y q49J · Dueño: Mantenimiento
Contraintuitivo: cinco de 22 equipos concentran el 40 % de la pérdida, y el peor de Planta 1 apenas suma 5,75 horas. Los diez primeros suman 575,25 de las 859 horas (67 %) y ocho de ellos son de los diez que se paran juntos el 7 de junio.

### I5 · Cuatro inversores de Planta 2 no reportan del 19 al 29 de mayo
Palanca: eficiencia y estado de los inversores
Evidencia: DESCRIPTIVO
Soporte: informe de análisis (los cuatro inversores sin registro)
Solidez: segmento sí · tiempo sí · atípicos no aplica · n = 904 tramos sin lectura en cada uno de los cuatro equipos
Impacto: hasta 297 MWh sobre los 785 MWh, solo si estuvieran parados. Es una cota superior y no entra en el business case.
Decisión: pedir al equipo de operación si esos cuatro equipos estaban parados o solo sin comunicación · Dueño: Operación
Contraintuitivo: no son tramos sueltos sino diez días seguidos, y dos de los cinco peores días de Planta 2 caen dentro de ese periodo.

## Lo que los datos no permiten decir

Los datos muestran qué inversores se paran y cuándo. No muestran por qué. No hay registro de alarmas, de cortes de red ni de órdenes de limitación de la planta, y esas causas no se pueden confirmar ni descartar con estas columnas. La energía perdida es una estimación con la mediana de los equipos vecinos, no una medida.
