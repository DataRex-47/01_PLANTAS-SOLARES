# Diseño del Proyecto

## Objetivos

Analizar los datos disponibles para intentar intuir donde pueden estar los problemas y si es necesario desplazar o
no a un equipo de ingenieros a las plantas.

![Plantas Solares](01_imagenes/Plantas%20Solares.png)

-----------------------------------------------------------------------------------------------
El proceso funciona de la siguiente manera :

La luz solar llega a los paneles solares, que producen corriente continua **DC**.   por medio de un dispositivo llamado ''inverter'' **transforma/convierte** la corriente **DC** en corriente alterna **AC**, que es la forma utilizada por la mayoría de los equipos eléctricos y por la red electrica. Hay cierta eficiencia del ''inversor'' que viene de la division AC/DC * 100. Se compara la Potencia AC de salida y la potencia DC de entrada.

Los sensores miden las condiciones de operación, como la irradiancia y la **temperatura** de los paneles y **ambientales**. El medidor registra la energía eléctrica entregada a la instalación o a la red. Estos datos se comparan para detectar si la planta produce menos de lo esperado.

Luego pasa a las redes por medio de esos palos donde pasa la electrecidad


## Palancas

Estas mueven el proceso de negocio

### Luz Solar - Irradiacion e Irradiancia (Palanca 1)

**Irradiacion** : Es la energia solar acumulada por unidad de superficie durante un periodo **kWh/m²**
Kilovatios por metro cuadrado **durante un periodo**.

A mayor irradiacion, mayor DC generada pero no es monotonico, llega un momento que el panel no recibe mas.

**Irradiancia** : Indica cuanta potencia solar recibe cada metro cuadrado de la planta en ese instante
**W/m²** kilovatios por metro cuadrado

### Estado de los Paneles(Palanca 2)

deben estar limpios y con un correcto funcionamiento para generar la mayor energía DC posible

### Eficiencia de los inverters:
siempre hay una pérdida en la transformación de DC a AC, pero debe ser la mínima posible. También deben estar en correcto estado y funcionamiento.

**DC (corriente continua)**: fluye en una sola dirección. Los paneles producen electricidad DC. Se mide como potencia en W o kW; también puede describirse con voltaje en V y corriente en A.

**AC (corriente alterna)**: cambia de dirección periódicamente. El inversor convierte la electricidad DC de los paneles en AC para alimentar equipos o entregar energía a la red. También se mide en W o kW (potencia), V y A.

Este es un inverter

![Plantas Solares](01_imagenes/inverter.jpg)

### Medidores y sensores:
si se estropean y no miden bien perdemos la trazabilidad y la posibilidad de detectar fallos.

Los sensores miden las condiciones de operación, como la irradiancia y la **temperatura** de los paneles y **ambientales**. El medidor registra la energía eléctrica entregada a la instalación o a la red

Esto es un Medidor y un sensor, Normalmente la planta - la instalacion completa en el terreno esta montado directamente sobre la estructura de soporte de los paneles

Estos son

![Plantas Solares](01_imagenes/Sensor_Medidor.png)

### Entidades

**DC** :

---------------

## KPI

-Irradiacion,kilowatios por metro cuadrado durante un periodo
-Eficiencia del inverter, AC/DC * 100
-Potencia DC, medida en Kw de corriente continua
-Potencia AC, medida en kw de corriente alterna
-Temperatura ambiente del modulo, medida en grados Celsius por el Sensor


## ENTIDADES

No se trata de cliente, producto, proveedor, tienda, etc. Pero en este caso estos serian :

Celda -> Modulos -> Paneles -> Arrays -> Inversores -> Planta

Con varios sensores y medidores, como explica la imagen

![Plantas Solares](01_imagenes/Planta_Entidades.png)

## Granularidad

De acuerdo a los datos de entrada:

Planta1_Generacion.csv
Planta1_Sensores.csv
Planta2_Generacion.csv
Planta2_Sensores.csv

- Se tienen dos plantas con **ventanas temporales** de 15 minutos durante 34 dias como el grano de cada fila,
- 2 plantas
- Inverters: varios por planta
- Sólo un sensor de irradiación por *Planta*
- Sólo un sensor de temperatura ambiente por *Planta*
* Sólo un sensor de temperatura del módulo por *Planta*

### limitacion

Al tener los sensores climáticos a nivel de toda la *Planta* y la medición eléctrica solo hasta cada *Inversor*, podemos identificar qué inversor baja su rendimiento, pero es imposible saber qué componente específico lo provoca aguas abajo, ya que no hay "ojos" en cada array, módulo o celda (suciedad, sombra local o daño).

Esto es exactamente lo mismo que ocurre en otros sectores:

En Retail: Sabes cuántas ventas registró cada Caja de la Tienda, pero sin el detalle transaccional por pasillo o producto, no puedes saber qué artículo específico dejó de venderse.

En Banca: Ves el balance de la Sucursal y la cartera total de cada Ejecutivo, pero sin el detalle por cliente o contrato, no puedes detectar qué producto financiero concreto está generando la pérdida.

Básicamente: "Diagnóstico a mitad de camino". Sabemos qué inversor rinde menos, pero sin el dato fino (grano) de las entidades inferiores, no es posible encontrar la causa raíz.


![Plantas Solares](01_imagenes/granularidad.png)


## Preguntas Semilla

Habiendo entendido las palancas, kpis y entidades ya podemos plantear las preguntas semilla:

Sobre la irradiación:

* ¿Llega suficiente irradiación todo los días?
* ¿Es similar en ambas plantas?
* ¿Cómo es su distribución por hora?
* ¿Cómo se relaciona con la temperatura ambiente y la temperatura del módulo?

Sobre las plantas:

* ¿Les llega la misma cantidad de irradiación?
* ¿Tienen similar número de inverters?
* ¿Generan similar cantidad de DC?
* ¿Generan similar cantidad de AC?

Sobre la generación de DC:

* ¿Cual es la relación entre irradiación y generación de DC?
* ¿Se ve afectada en algún momento por la temperatura ambiente o del módulo?
* ¿Es similar en ambas plantas?
* ¿Cómo se distribuye a lo largo del día?
* ¿Es constante a lo largo de los días?
* ¿Es constante en todos los inverters?
* ¿Ha habido momentos de fallos?

Sobre la generación de AC:

* ¿Cual es la relación entre DC y generación de AC?
* ¿Es similar en ambas plantas?
* ¿Cómo se distribuye a lo largo del día?
* ¿Es constante a lo largo de los días?
* ¿Es constante en todos los inverters?
* ¿Ha habido momentos de fallos?

Sobre los medidores y sensores:

* ¿Son fiables los datos de irradiación?
* ¿Son fiables los datos de temperatura?
* ¿Son fiables los datos de DC?
* ¿Son fiables los datos de AC?
* ¿Son similares los datos entre ambas plantas?