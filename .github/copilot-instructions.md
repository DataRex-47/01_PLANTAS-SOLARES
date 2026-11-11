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

**Fase actual**: proyecto creado, falta B_01_DisenoProyecto
**Objetivo de negocio**: (pendiente)
**Unidad de análisis**: (pendiente)
**Palancas elegidas**: (pendiente)
**Documentos vivos**: (ninguno)
**Fuentes limpias**: (todavía no hay)
**Dataframe actual**: (todavía no hay)
**Estructura del dataframe**: (todavía no hay)
**Preguntas**: 0 respondidas · 0 pendientes
**Insights validados**: 0
