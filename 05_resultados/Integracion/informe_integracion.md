# Integración

## 1. Fuentes y unión

Se concatenaron las dos fuentes de generación, con 136.476 filas, y las dos fuentes de sensores, con 6.441 filas. La generación se unió con los sensores mediante `left` por `fecha_hora` e `id_planta`, conservando el lado de generación.

El notebook no registra las filas ni la suma de la métrica clave antes y después de la unión. Tampoco establece `validate` ni reporta claves sin correspondencia. Por ello, la unión está ejecutada, pero su suma de control no queda documentada.

## 2. Resumen diario

Se resumieron las variables por `id_planta`, `id_inversor` y día. Se calcularon mínimo, media y máximo para temperaturas, potencias, eficiencia y energía diaria; para irradiación también se calculó la suma. Los nombres de columnas se aplanaron.

El resumen diario por planta, inversor y día tiene 1.496 filas: 44 inversores por 34 días, del 15 de mayo al 17 de junio de 2020. Los días sin lectura de un inversor quedan con valores vacíos. Las sumas de irradiación de esos días no deben leerse como irradiación observada.

## 3. Resultado y pendientes

El notebook guarda `tablon_analitico.pickle` y `tablon_analitico_diario` en `02_datos/02_Analitico`.

No queda registrada una comprobación de unicidad para planta, inversor y día ni una suma de control final contra las fuentes. Esas validaciones quedan pendientes antes de considerar certificado el tablón diario.
