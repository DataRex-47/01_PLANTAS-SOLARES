# Análisis

**Resultado:** los días y los equipos donde las dos plantas pierden energía.

## 1. Qué se analizó

Se trabajó sobre el tablón de 15 minutos de las dos plantas, 34 jornadas entre el 15 de mayo y el 17 de junio de 2020, con 22 inversores por planta. Se revisaron cuatro frentes: captación de irradiación, generación DC y AC, forma de la curva horaria y disponibilidad de cada inversor.

## 2. Captación

| Indicador | Planta 1 | Planta 2 |
|---|---|---|
| Irradiación media con sol | 0,42 | 0,37 |
| Temperatura ambiente media (°C) | 25,92 | 27,41 |
| Temperatura de módulo media (°C) | 31,80 | 31,56 |

Planta 2 recibe 12 % menos irradiación y tiene 1,49 °C más de temperatura ambiente, pero la temperatura de módulo es prácticamente igual, con 0,24 °C de diferencia. Esa combinación es posible porque el módulo depende del viento y del montaje, no solo del aire.

No se observa una señal de fallo de captación. La diferencia de irradiación es compatible con la ubicación de cada planta, no con un sensor descalibrado.


## 3. Generación DC y AC

| Comparación | Lectura |
|---|---|
| Irradiación contra potencia DC | relación lineal hasta el codo de la planta, sin saturación temprana |
| Irradiación contra potencia AC | misma forma que la DC, con la pérdida de conversión repartida |
| Puntos fuera de la nube | se concentran en Planta 2, con irradiación alta y potencia baja |

Los puntos que se separan de la nube no siguen la irradiación. Esa forma, con sol disponible y potencia en cero o muy baja, corresponde a un inversor detenido y no a una nube de paso.

## 4. Parones por inversor

Para no depender de la impresión visual se contó, en cada jornada, cuántos tramos de 15 minutos tiene un inversor en cero mientras el resto de su planta está produciendo. La suma se pasa a horas. La noche queda fuera por construcción, porque solo se miran los tramos en que el parque genera. Los equipos sin registros tampoco suman horas.

Cinco días con más inversor-horas caídas por planta:

| Planta | Fecha | Inversores | Tramos caídos | Inversor-horas caídas |
|---|---|---|---|---|
| Planta 1 | 2020-06-07 | 22 | 33 | 8,25 |
| Planta 1 | 2020-06-14 | 22 | 24 | 6,00 |
| Planta 1 | 2020-05-27 | 22 | 5 | 1,25 |
| Planta 1 | 2020-05-15 | 22 | 3 | 0,75 |
| Planta 1 | 2020-05-18 | 22 | 2 | 0,50 |
| Planta 2 | 2020-06-07 | 22 | 202 | 50,50 |
| Planta 2 | 2020-06-06 | 22 | 197 | 49,25 |
| Planta 2 | 2020-06-08 | 22 | 177 | 44,25 |
| Planta 2 | 2020-05-22 | 18 | 176 | 44,00 |
| Planta 2 | 2020-05-23 | 18 | 158 | 39,50 |

Cinco inversores con más energía perdida por planta:

| Planta | Inversor | Parones | Horas paradas | kWh perdidos |
|---|---|---|---|---|
| Planta 2 | Et9kgGMDl729KT4 | 43 | 74,00 | 69.646,8 |
| Planta 2 | LYwnQax7tkwH5Cb | 29 | 72,00 | 68.752,2 |
| Planta 2 | Quc1TzYxW2pYoWX | 48 | 68,50 | 65.387,7 |
| Planta 2 | rrq4fwE8jgrTyWY | 34 | 62,00 | 58.731,7 |
| Planta 2 | q49J1IKaHRwDQnt | 28 | 54,25 | 52.544,4 |
| Planta 1 | bvBOhCH3iADSZry | 6 | 5,75 | 4.958,4 |
| Planta 1 | 1BY6WEcLGh8j5v7 | 5 | 5,25 | 4.627,8 |
| Planta 1 | z9Y9gH1T5YWrNuG | 2 | 1,75 | 1.833,5 |
| Planta 1 | wCURE6d3bPkepu2 | 1 | 1,25 | 1.519,7 |
| Planta 1 | McdE0feGgRqW7Ca | 1 | 0,75 | 882,2 |

La energía perdida se estima con la mediana de los inversores vecinos en ese mismo instante, así que es un orden de magnitud y no una medida exacta.

## 5. Cómo se clasifica cada lectura

Cada bloque de 15 minutos de cada inversor recibe una de cuatro etiquetas. La regla se aplica siempre contra la mediana de los inversores de su planta en ese mismo instante, no contra la hora anterior ni la siguiente.

| Etiqueta | Regla | Qué significa |
|---|---|---|
| operativo | produce al menos el 70 % de la mediana de su planta | comportamiento normal |
| potencia_baja | produce algo, pero menos del 70 % de la mediana de su planta | el equipo entrega menos de lo que le toca |
| potencia_cero | marca cero mientras la mediana de su planta es positiva | el equipo está detenido con sol disponible |
| sin_sol | la mediana de su planta es cero | es de noche, no se juzga |
| sin_dato | no hay lectura del parque en ese instante | no se puede decidir |

El 70 % se eligió como umbral de criba, no como una verdad física. Sirve para separar lo que se sale de la banda del parque de lo que va en línea con él. Una nube pasando sobre una hilera puede disparar `potencia_baja` en un tramo suelto, así que la etiqueta solo se usa para encontrar equipos recurrentes, no para afirmar que ese tramo fue una avería.

Comparar contra la mediana del parque en el mismo instante, y no contra las horas vecinas del propio equipo, evita dos errores clásicos: al amanecer y al atardecer los vecinos también están casi en cero, y una nube que tapa media planta afecta igual a todos sus inversores.

El detalle completo se guarda en `05_resultados/Analisis/fallos.csv`, una fila por inversor y bloque de 15 minutos, con planta, fecha, inversor, hora, potencia y etiqueta.

## 6. Días que pasan al detalle

| Planta | Fechas |
|---|---|
| Planta 1 | 2020-05-15, 2020-05-18, 2020-05-20, 2020-05-27, 2020-06-07, 2020-06-14 |
| Planta 2 | 2020-05-22, 2020-05-23, 2020-06-06, 2020-06-07, 2020-06-08 |

El 20 de mayo de Planta 1 entra por revisión manual. La media horaria suaviza las caídas cortas y ese día la curva se corta entre las 14 y las 16, con 76 de 96 ventanas registradas.

## 7. Conclusión

La captación no explica las pérdidas. El problema está en la disponibilidad de los inversores, y se concentra en Planta 2.

Planta 2 pierde entre cinco y seis veces más inversor-horas que Planta 1 en sus peores días. Sus tres peores jornadas son consecutivas, del 6 al 8 de junio.

El 7 de junio se revisó hora por hora. Entre las 10 y las 14, Planta 2 tuvo entre 6 y 10 de sus 22 inversores en cero la hora completa, mientras Planta 1 no pasó de un equipo en un solo tramo. El parón es de Planta 2 y no de las dos plantas, y el patrón apunta a un grupo de equipos que se detiene junto.

Cinco equipos de Planta 2 suman entre 62 y 74 horas paradas en 34 días, con 28 a 48 caídas cada uno. En Planta 1 el peor equipo llega a 5,75 horas. La diferencia es de orden.

## 8. Preguntas abiertas

| Id | Pregunta | Señal que la enciende |
|---|---|---|
| P1 | ¿Los cinco inversores de Planta 2 comparten alimentación o comunicaciones? | 28 a 48 caídas por equipo en 34 días |
| P2 | ¿Por qué entre las 10 y las 14 del 7 de junio se detuvieron entre 6 y 10 inversores de Planta 2? | revisión hora por hora del día |
| P3 | ¿Qué cuatro inversores de Planta 2 no reportan el 21, 22, 26, 27 y 28 de mayo? | 18 de 22 inversores en esos cinco días |


Ninguna se responde en este informe. Quedan anotadas para la revisión con el equipo de operación.
