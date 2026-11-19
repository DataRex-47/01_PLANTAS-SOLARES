# METADATOS DEL PROYECTO

## DATOS DE GENERACIÓN

**fecha_hora**: Fecha y hora del registro. Las mediciones se realizan a intervalos de 15 minutos.

**id_planta**: Identificador único de la planta solar. Es constante en todo el archivo.

**id_inversor**: Identificador único del inversor al que pertenece el registro.

**potencia_dc_kw**: Potencia instantánea en corriente continua (DC) generada por el inversor en el momento de la medición. Unidad: kW.

**potencia_ac_kw**: Potencia instantánea en corriente alterna (AC) generada por el inversor en el momento de la medición. Unidad: kW.

**energia_diaria_kwh**: Energía acumulada generada durante el día actual, reiniciada a cero al inicio de cada día. Unidad: kWh.

[Cuidado con la diferencia de instantanea vs acumulada]

**energia_total_kwh**: Energía acumulada total generada por el inversor desde su instalación hasta el momento del registro. Unidad: kWh.

---

## DATOS DE SENSORES

**fecha_hora**: Fecha y hora del registro. Las mediciones se realizan a intervalos de 15 minutos.

**id_planta**: Identificador único de la planta solar. Es constante en todo el archivo.

**id_sensor_meteorologico**: Identificador del sensor meteorológico de la planta. Dado que hay un único sensor por planta, este valor es constante.

**temperatura_ambiente_c**: Temperatura ambiente en la planta. Unidad: °C.

**temperatura_modulo_c**: Temperatura del módulo (panel solar) conectado al sensor. Unidad: °C.

**irradiacion_wh_m2**: Energía solar recibida acumulada durante el intervalo de 15 minutos. Unidad: Wh/m² (vatios hora por metro cuadrado).
No confundir con irradiancia (W/m²), que es una medida instantánea.

---

## NOTAS

### Potencia vs energía

**Aplica a:** `potencia_dc_kw`, `potencia_ac_kw`, `energia_diaria_kwh`, `energia_total_kwh`.

**Detalle:**
Potencia es la cantidad de energía generada o consumida en un instante.
Energía es la cantidad total generada o consumida en un periodo temporal.
Energía = Potencia x Tiempo

La potencia se expresa en kW y es instantánea (valor en el momento de la medición).
La energía se expresa en kWh y representa una acumulación a lo largo del tiempo.

---

### Energía acumulada

**Aplica a:** `energia_diaria_kwh`, `energia_total_kwh`.

**Detalle:** Son acumulados:
- La **diaria** se reinicia cada día.
- La **total** crece históricamente y **no se reinicia**.

---

### Irradiación vs irradiancia

**Aplica a:** `irradiacion_wh_m2`.

**Detalle:**
- **Irradiancia (W/m²)**: Potencia instantánea.
- **Irradiación (Wh/m²)**: Energía recibida en un intervalo (15 min aquí).


Este dataset contiene **irradiación**.

---

### Intervalo temporal de medición

**Aplica a:** `fecha_hora` de ambos ficheros y a todas las variables medidas.

**Detalle:**
La frecuencia de muestreo es de **15 minutos**.
- Las variables de **potencia** son instantáneas en ese *timestamp*.
- Las de **energía** e **irradiación** corresponden a un acumulado hasta ese punto (o dentro del intervalo, según el caso).

---

## NOTAS DEL PROYECTO

### Irradiación: qué unidad se usa

**Aplica a:** `irradiacion_wh_m2`.

**Detalle:** el diccionario original declara Wh/m² por tramo de 15 minutos. Los valores observados, con media de 0,23 y máximo de 1,22, no pueden ser Wh/m² de un tramo de 15 minutos, porque el sol pleno aporta unos 250 Wh/m² en ese tiempo. Cuadran con **kW/m² instantáneos**, donde 1,0 es sol pleno. Con esa lectura la irradiación diaria suma unos 5,5 kWh/m², coherente con mayo y junio. En el proyecto se trata como kW/m² y queda marcado como supuesto. Si se sumara la columna tal como está, el resultado quedaría unas 250 veces corto.

### Unidades de energía

| Unidad | Qué es | Equivalencia |
|---|---|---|
| kW | potencia, lo que produce un equipo en un instante | |
| kWh | energía, potencia por tiempo | 1 kW durante 1 hora |
| MWh | mil kWh, la unidad en que se reporta la energía de una planta | 1 MWh = 1.000 kWh |

### Términos del análisis

| Término | Qué es |
|---|---|
| Tramo | una lectura cada 15 minutos |
| Inversor caído | en un tramo, marca 0 kW de AC mientras la mediana de su planta, en ese mismo instante, es mayor que 0 |
| Inversor hora | cuatro tramos caídos, es decir, un inversor parado durante una hora |
| Energía perdida | lo que habría producido un inversor caído, estimado con la mediana de sus vecinos en ese instante, por 0,25 horas por tramo |
| Energía recuperable | la parte de la energía perdida que se vuelve a producir al reparar los equipos; es un supuesto, no un dato |

