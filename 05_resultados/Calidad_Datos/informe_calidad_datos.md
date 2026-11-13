# Calidad de datos

Las cuatro fuentes entran completas, sin nulos y sin duplicados, con dos defectos reales corregidos o marcados: la potencia DC de planta 1 venía 10 veces alta y el contador de energía diaria no cambia de día a medianoche. La serie no se regulariza y no se imputa nada.

## 1. Fuentes importadas

| Nombre | Fichero | Filas | Columnas | Grano | Clave candidata |
|---|---|---|---|---|---|
| p1g | Planta1_Generacion.csv | 68.778 | 7 | 15 minutos por inversor | fecha_hora + id_inversor |
| p2g | Planta2_Generacion.csv | 67.698 | 7 | 15 minutos por inversor | fecha_hora + id_inversor |
| p1s | Planta1_Sensores.csv | 3.182 | 6 | 15 minutos por planta | fecha_hora |
| p2s | Planta2_Sensores.csv | 3.259 | 6 | 15 minutos por planta | fecha_hora |

Lo que hubo que resolver en la carga: separador coma, sin comillas ni nulos, y dos formatos de fecha, porque p1g escribe `15-05-2020 00:00` y las otras tres `2020-05-15 00:00:00`. Se cargan como texto y cada fuente se convierte con su formato explícito, p1g en `%d-%m-%Y %H:%M` y las otras tres en `%Y-%m-%d %H:%M:%S`. `id_planta` llega como número en las cuatro fuentes (4135001 y 4136001).

## 2. Problemas por dimensión

### Completitud

| Fuente | Filas | Nulos | Ventanas únicas | Ventanas esperadas | Faltan | Días incompletos | Mínimo por día |
|---|---|---|---|---|---|---|---|
| p1g | 68.778 | 0 | 3.158 | 3.264 | 106 (3,2 %) | 10 | 65 |
| p2g | 67.698 | 0 | 3.259 | 3.264 | 5 (0,2 %) | 5 | 95 |
| p1s | 3.182 | 0 | 3.182 | 3.264 | 82 (2,5 %) | 8 | 68 |
| p2s | 3.259 | 0 | 3.259 | 3.264 | 5 (0,2 %) | 5 | 95 |

Las cuatro fuentes cubren 34 jornadas y ninguna es regular. Faltan tramos dentro del día, no días enteros. El caso más claro es el 21 de mayo en planta 1: generación trae 65 ventanas de 96 y sensores 68. No hay nulos encubiertos (ni vacíos ni `N/A`) en los ficheros originales.

Decisión: los huecos se dejan como están. Interpolar potencia es inventar generación, y el análisis compara siempre los mismos días.

### Validez

| Comprobación | Resultado | Acción |
|---|---|---|
| Valores negativos | 0 filas | nada |
| Potencia AC positiva con DC igual a 0 | 0 filas | nada |
| Fechas fuera de 2020 | 0 filas | nada |
| potencia_dc_kw de planta 1 | media 3.147,43 kW frente a 307,80 kW de AC, eficiencia 9,8 % | dividida entre 10 |
| energia_diaria_kwh a las 00:00 | 161 de 681 arranques en p1 y 516 de 702 en p2 con valor distinto de cero | se conserva y se marca |
| energia_total_kwh de planta 2 | de 1,7 a 2.247,9 millones de kWh entre inversores | se conserva, no sirve para comparar equipos |

El contador diario no cambia de día a medianoche. El inversor `4UPUqMRk7TRMgml` de planta 2 arrastra 9.425 kWh a las 00:00, 00:15, 00:30 y 00:45 del 15 de mayo, baja a 7.540 a la 01:00 y a 0 a la 01:15. Sumar la columna por día atribuye energía al día equivocado.

### Unicidad

| Fuente | Duplicados completos |
|---|---|
| p1g | 0 |
| p2g | 0 |
| p1s | 0 |
| p2s | 0 |

En los dos sensores el número de filas coincide con el número de marcas únicas, así que no hay dos lecturas para la misma hora.

### Consistencia

| Problema | Detalle | Acción |
|---|---|---|
| Misma planta con dos códigos | 4135001 y 4136001 en las cuatro fuentes | pasan a Planta 1 y Planta 2, 142.917 filas |
| Dos formatos de fecha | dd-mm-aaaa en p1g, ISO en las otras tres | formato explícito por fuente, 142.917 filas |
| Unidad de irradiación | declarada Wh/m², pero su nivel (media 0,23, máximo 1,22) corresponde a kW/m² instantáneos | se conserva y queda marcada como supuesto |

La irradiación, tomada como está, integra 5,5 kWh/m² al día, coherente con mayo y junio. Si se sumara la columna para obtener la irradiación diaria, el resultado quedaría unas 250 veces corto. Es lo primero que hay que cerrar antes de cruzar irradiación con generación.

### Actualidad

| Concepto | Dato |
|---|---|
| Periodo | 15 de mayo a 17 de junio de 2020, 34 jornadas |
| Distancia a hoy | 6 años y 5 meses |
| Marcas desalineadas de la rejilla de 15 minutos | 0 |

La tabla de regularidad fechó primero p2g, p1s y p2s entre el 6 de enero y el 6 de diciembre, lo que estiraba la rejilla teórica a 32.256 ventanas y 28.997 faltantes. La causa era la conversión de fechas: el parseo mixto con día primero invertía las cadenas ISO, y los días 01-06 a 12-06 quedaban como 6 de enero, 6 de febrero y así hasta 6 de diciembre, doce días falsos que sumaban los 45 días vistos en el análisis.

Se corrigió fijando el formato de cada fuente. Las cuatro quedan entre el 15 de mayo y el 17 de junio de 2020, la rejilla teórica baja a 3.264 ventanas y los días pasan de 45 a 34. Después de la corrección hay que volver a correr la limpieza, la integración y el análisis en ese orden, porque los ficheros intermedios guardados conservaban las fechas viejas.

## 3. Correcciones aplicadas

| Corrección | Filas | Antes | Después |
|---|---|---|---|
| potencia_dc_kw de planta 1 dividida entre 10 | 68.778 | media 3.147,43 kW, eficiencia AC/DC 9,8 % | media 314,74 kW, eficiencia 97,8 % |
| id_planta a texto | 142.917 | 4135001 y 4136001 | Planta 1 y Planta 2 |
| fecha_hora a fecha y hora | 142.917 | texto con dos formatos | datetime |

Eficiencia de conversión por planta, calculada sobre la media y no fila a fila: planta 1 pasa de 9,8 % a 97,8 % y planta 2 se queda en 97,8 %. La regla de negocio marca alarma por debajo del 93 %.

Nada se elimina ni se imputa: 0 filas fuera y 0 kWh fuera del análisis.

## 4. Lo que quedó fuera

198 ventanas de 15 minutos sin lectura (106 en p1g, 82 en p1s y 5 en cada fuente de planta 2). No se rellenan ni se estima su valor. Tampoco se regulariza la serie: el análisis trabaja con las ventanas observadas.

Queda pendiente de comprobar, y no se ha usado todavía, el duplicado por clave en generación (fecha e inversor), porque el notebook solo miró filas completas.

## 5. Sorpresas y preguntas propuestas

| Id | Pregunta | Dato que la enciende | Hipótesis | Prioridad |
|---|---|---|---|---|
| P0.1 | ¿Por qué planta 1 pierde tramos en 10 días y llega a 65 de 96 ventanas el 21 de mayo? | 106 ventanas sin dato y todos los inversores afectados por igual | fallo de captura o de guardado, no de un equipo | Alta |
| P0.2 | ¿Por qué cuatro inversores de planta 2 traen unas 3.074 filas frente a 3.259 del resto? | conteo de registros por inversor | comunicaciones intermitentes de esos equipos | Alta |
| P0.3 | ¿Por qué el contador diario de energía no vuelve a cero a medianoche? | 161 y 516 arranques de día con valor distinto de cero | desfase del contador, el día cierra a la 01:15 | Media |
| P0.4 | ¿La irradiación está en kW/m² o en Wh/m²? | media 0,23 y máximo 1,22 | la columna trae irradiancia instantánea y el diccionario dice energía | Alta |
| P0.5 | ¿Hay inversores o días por debajo del 93 % de eficiencia? | media de 97,8 % en las dos plantas frente al umbral de negocio del 93 % | pérdida sostenida en algunos equipos | Alta |
| P0.6 | ¿Por qué la energía total de planta 2 va de 1,7 a 2.247,9 millones de kWh? | desviación de 729,7 millones entre filas | contadores con distinta fecha de puesta en marcha | Baja |

Ninguna se investiga aquí. Quedan anotadas a la espera de tu aprobación.

## 6. Ficheros guardados

| Fichero | Filas | Columnas |
|---|---|---|
| 02_datos/02_Analitico/Planta1_Generacion.pkl | 68.778 | 7 |
| 02_datos/02_Analitico/Planta2_Generacion.pkl | 67.698 | 7 |
| 02_datos/02_Analitico/Planta1_Sensores.pkl | 3.182 | 6 |
| 02_datos/02_Analitico/Planta2_Sensores.pkl | 3.259 | 6 |

Pasa a integración un conjunto de cuatro fuentes sin imputar, con la potencia DC de planta 1 corregida y con los días incompletos y los cuatro inversores de planta 2 marcados.
