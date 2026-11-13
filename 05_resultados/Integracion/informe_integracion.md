# Integración

## 1. Fuentes y unión

Se concatenaron las dos fuentes de generación, con 136.476 filas, y las dos fuentes de sensores, con 6.441 filas. La generación se unió con los sensores mediante `left` por `fecha_hora` e `id_planta`, conservando el lado de generación.

El notebook no registra las filas ni la suma de la métrica clave antes y después de la unión. Tampoco establece `validate` ni reporta claves sin correspondencia. Por ello, la unión está ejecutada, pero su suma de control no queda documentada.

## 2. Resumen diario

Se resumieron las variables por `id_planta`, `id_inversor` y día. Se calcularon mínimo, media y máximo para temperaturas, potencias, eficiencia y energía diaria; para irradiación también se calculó la suma. Los nombres de columnas se aplanaron.

La salida de `df.info()` muestra 8.140 filas y 24 columnas, con índice diario entre el 15 de mayo y el 6 de diciembre de 2020. Las estadísticas de generación tienen 1.464 valores no nulos y las de sensores 1.222. `irradiacion_wh_m2_sum` aparece con 8.140 valores no nulos. Como `resample('D')` genera días sin lecturas y la suma puede devolver cero en esos grupos, esos valores no deben interpretarse automáticamente como irradiación observada.

## 3. Resultado y pendientes

El notebook guarda `tablon_analitico.pickle` y `tablon_analitico_diario` en `02_datos/02_Analitico`.

No queda registrada una comprobación de unicidad para planta, inversor y día ni una suma de control final contra las fuentes. Esas validaciones quedan pendientes antes de considerar certificado el tablón diario.
