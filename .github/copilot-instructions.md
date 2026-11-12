1. Responde siempre en español, tuteando (nunca vos).
2. Comienza cada mensaje con el emoji 🤖
3. Genera el código para el notebook abierto, nunca para la ventana interactiva, salvo que se indique lo contrario.
4. No expliques el código salvo que se pida.

## Tipo de proyecto
Business Analytics (Discovery). Pipeline de agentes: B_01_DisenoProyecto, B_02_MapaEntidades, B_03_PreguntasSemilla, B_04_CalidadDatos, B_05_Integrador, B_06_Analista, B_07_Insights, B_08_Exhibits, B_09_BusinessCase y, si algo se va a seguir en el tiempo, B_10_ArquitectoDashboard.

## Convenciones de este proyecto
1. Entorno con **UV**: `uv add <paquete>`, nunca `pip install`.
2. **Bucles tradicionales** (`for` + `append`) en lugar de comprehensions y lambdas.
3. Comentarios de sección `# ── Título ─────────`; comentarios cortos en minúscula que expliquen el porqué.
4. Rutas siempre con `pathlib`.
5. Los datos de `02_datos/01_Originales/` son de **solo lectura**.
6. Cada unión de tablas se comprueba con una suma de control de la métrica clave antes y después.
7. **Funciones reutilizables**: antes de escribir una, comprobar `04_scripts/utils.py`. Las gestiona `A_14_Bibliotecario`.
8. Resultados siempre como DataFrame o tabla Markdown, nunca como diccionarios.
9. Documentos e informes: sin guiones ni rayas, sin barras en rótulos dobles, sin subtítulos en gris; cortos, centrados en qué pasó y qué se decidió, no en teoría.

## ESTADO ACTUAL DEL PROYECTO

**Fase actual**: Integración ejecutada; pendientes la suma de control de la unión y la comprobación de unicidad del grano diario
**Objetivo de negocio**: detectar dónde pierden las dos plantas y decidir si hace falta desplazar a un equipo de ingenieros
**Unidad de análisis**: inversor y ventana de 15 minutos en generación, planta y ventana de 15 minutos en sensores
**Palancas elegidas**: irradiación, estado de los paneles, eficiencia de los inversores, medidores y sensores
**Documentos vivos**: 06_resultados/Calidad_Datos/informe_calidad_datos.md · 06_resultados/Integracion/informe_integracion.md
**Fuentes limpias**: 02_datos/02_Analitico/Planta1_Generacion.pickle · 02_datos/02_Analitico/Planta2_Generacion.pickle · 02_datos/02_Analitico/Planta1_Sensores.pickle · 02_datos/02_Analitico/Planta2_Sensores.pickle
**Dataframe actual**: 02_datos/02_Analitico/tablon_analitico.pickle · 02_datos/02_Analitico/tablon_analitico_diario
**Estructura del dataframe**: salida capturada de `df.info()`

```text
<class 'pandas.DataFrame'>
DatetimeIndex: 8140 entries, 2020-05-15 to 2020-12-06
Data columns (total 24 columns):
 #   Column                       Non-Null Count  Dtype
---  ------                       --------------  -----
 0   id_planta                    8140 non-null   str
 1   id_inversor                  8140 non-null   str
 2   irradiacion_wh_m2_min        1222 non-null   float64
 3   irradiacion_wh_m2_mean       1222 non-null   float64
 4   irradiacion_wh_m2_max        1222 non-null   float64
 5   irradiacion_wh_m2_sum        8140 non-null   float64
 6   temperatura_ambiente_c_min   1222 non-null   float64
 7   temperatura_ambiente_c_mean  1222 non-null   float64
 8   temperatura_ambiente_c_max   1222 non-null   float64
 9   temperatura_modulo_c_min     1222 non-null   float64
 10  temperatura_modulo_c_mean    1222 non-null   float64
 11  temperatura_modulo_c_max     1222 non-null   float64
 12  potencia_dc_kw_min           1464 non-null   float64
 13  potencia_dc_kw_mean          1464 non-null   float64
 14  potencia_dc_kw_max           1464 non-null   float64
 15  eficiencia_min               1464 non-null   float64
 16  eficiencia_mean              1464 non-null   float64
 17  eficiencia_max               1464 non-null   float64
 18  potencia_ac_kw_min           1464 non-null   float64
 19  potencia_ac_kw_mean          1464 non-null   float64
 20  potencia_ac_kw_max           1464 non-null   float64
 21  energia_diaria_kwh_min       1464 non-null   float64
 22  energia_diaria_kwh_mean      1464 non-null   float64
 23  energia_diaria_kwh_max       1464 non-null   float64
dtypes: float64(22), str(2)
memory usage: 1.6 MB
```
**Preguntas**: 0 respondidas · 6 pendientes (P0.1 a P0.6 propuestas en el informe, pendientes de aprobación)
**Insights validados**: 0
