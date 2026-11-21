# Predicción de la demanda energética de edificios con Machine Learning

**Producto de la Unidad II | Ciencia de Datos e Inteligencia Artificial Aplicada**  
**Universidad Peruana Unión (UPeU) — Escuela de Posgrado**  
**Docente:** Doris Elena Campo Duarte  
**Integrantes:** *Esther Elen Tafura Alegria*  
**Fecha de entrega de referencia:** 22 de noviembre de 2026

## 1. ¿De qué trata este proyecto?

Este proyecto busca estimar las necesidades de **calefacción y refrigeración de un edificio** a partir de sus características arquitectónicas. Para ello, utilizamos una base de datos de edificios simulados y comparamos distintos modelos de regresión en Python.

El trabajo se desarrolla en **Google Colab**, dentro de un notebook que combina explicaciones metodológicas, código comentado, tablas, gráficos, resultados y conclusiones. Está preparado para que otro integrante del equipo o la docente pueda reproducir el análisis paso a paso.

**Pregunta de investigación:** ¿Qué modelo predictivo estima con menor error las cargas de calefacción y refrigeración a partir de las características arquitectónicas del edificio?

**Objetivo general:** comparar el desempeño de una regresión lineal, Random Forest, XGBoost y LightGBM, optimizar XGBoost y seleccionar el modelo con menor error de validación cruzada para cada variable objetivo.

> **Importante:** el estudio analiza escenarios **simulados**. Sus resultados no deben interpretarse como una validación automática para cualquier edificio real, ni como relaciones causales.

## 2. Dataset utilizado

- **Nombre:** *Energy Efficiency* (Eficiencia Energética).
- **Fuente original:** UCI Machine Learning Repository.
- **Autores del estudio original:** Athanasios Tsanas y Angeliki Xifara (2012).
- **Archivo de trabajo:** `Copia de ENB2012_data.xlsx`.
- **Ubicación del archivo:** [abrir Excel en Google Drive](https://docs.google.com/spreadsheets/d/1WGidEYywXMNfRjsxL-CKZtdbn8cK7pts/edit).
- **Identificador de archivo:** `1WGidEYywXMNfRjsxL-CKZtdbn8cK7pts`.
- **Tamaño:** 768 registros y 10 variables utilizadas.

| Variable | Descripción | Función |
|---|---|---|
| X1 | Compacidad relativa | Predictora |
| X2 | Área de superficie | Predictora |
| X3 | Área de paredes | Predictora |
| X4 | Área del techo | Predictora |
| X5 | Altura total | Predictora |
| X6 | Orientación del edificio | Predictora categórica |
| X7 | Área relativa de acristalamiento | Predictora |
| X8 | Distribución del acristalamiento | Predictora categórica |
| **Y1** | **Carga de calefacción** | **Objetivo 1** |
| **Y2** | **Carga de refrigeración** | **Objetivo 2** |

Las dos variables objetivo son numéricas: por eso resolvemos un **problema de regresión**, no de clasificación. Los valores de Y1 y Y2 se estiman en análisis separados utilizando únicamente las variables X1 a X8.

## 3. Archivos del proyecto

El archivo principal es el notebook:

```text
proyecto-eficiencia-energetica/
├── README.md
└── U2_Eficiencia_Energetica_Modelos_Predictivos_Colab.ipynb
```

El Excel se encuentra en Google Drive y **se lee desde su ubicación original**. No es necesario duplicarlo ni subirlo al repositorio para ejecutar el análisis, siempre que la cuenta de Google utilizada tenga permiso para acceder a él.

**Notebook de trabajo:** [abrir en Google Colab](https://colab.research.google.com/drive/1MJ1erLi_n7rZSaJQH4KB5NvEgjp2Z2Rk).

## 4. Cómo ejecutar el proyecto: instrucciones para cada usuario

### Paso 1. Preparar los accesos

Cada integrante necesita:

1. Una cuenta de Google con acceso a Google Colab.
2. **Permiso de lectura del Excel** `Copia de ENB2012_data.xlsx` en Drive. Tener acceso al notebook no garantiza automáticamente el acceso al dataset.
3. Conexión a Internet mientras se ejecuta el notebook.

**Comprobación rápida:** antes de comenzar, abrir el [archivo Excel](https://docs.google.com/spreadsheets/d/1WGidEYywXMNfRjsxL-CKZtdbn8cK7pts/edit). Si no se puede visualizar, solicitar permiso al propietario; de lo contrario, la lectura desde Python podría devolver un error de acceso.

### Paso 2. Abrir el notebook

Entrar al [notebook de Google Colab](https://colab.research.google.com/drive/1MJ1erLi_n7rZSaJQH4KB5NvEgjp2Z2Rk).

También se puede abrir una copia `.ipynb` desde [Google Colab](https://colab.research.google.com/) mediante **Archivo → Subir notebook**.

Si varias personas van a experimentar con el código, es recomendable que cada una use **Archivo → Guardar una copia en Drive**. Así no sobrescriben accidentalmente el trabajo compartido.

### Paso 3. Seleccionar el entorno

En Google Colab, verificar que el entorno de ejecución utilice **Python 3**. No se requiere GPU ni una computadora de alto rendimiento para este dataset.

La primera celda de código instala las bibliotecas externas que hacen falta:

```python
%pip -q install openpyxl xgboost lightgbm optuna
```

Las bibliotecas principales son `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `lightgbm` y `optuna`.

### Paso 4. Ejecutar las celdas en orden

En el menú superior, seleccionar **Entorno de ejecución → Ejecutar todas**.

Cuando Google Colab solicite autenticación, seleccionar la cuenta que tenga acceso al Excel y autorizar la lectura de Google Drive. El notebook utiliza `google.colab.auth`, la API de Google Drive y el ID del archivo para leer el contenido en memoria.

La instrucción de carga es equivalente a:

```python
ID_ARCHIVO = "1WGidEYywXMNfRjsxL-CKZtdbn8cK7pts"
contenido_excel = servicio_drive.files().get_media(fileId=ID_ARCHIVO).execute()
datos = pd.read_excel(io.BytesIO(contenido_excel), sheet_name=0)
```

**No hace falta cambiar el ID** mientras se utilice el Excel original del proyecto. Tampoco se necesita montar manualmente una carpeta `MyDrive` ni escribir una ruta local: el acceso se hace por identificador.

### Paso 5. Revisar los resultados

Después de ejecutar todas las celdas, verificar:

- Que se hayan leído **768 filas**.
- Que aparezcan las tablas de calidad y limpieza de datos.
- Que los gráficos exploratorios se visualicen correctamente.
- Que se muestre la tabla comparativa de modelos con **MAE, RMSE y R²**.
- Que se identifique el mejor modelo para **Y1** y para **Y2**.
- Que se generen las conclusiones a partir de los resultados calculados.

**No es necesario editar ni guardar cambios sobre el Excel original.** El notebook trabaja con copias de los datos en memoria.

## 5. Etapas del análisis

| Etapa | ¿Qué se realiza? | ¿Para qué sirve? |
|---|---|---|
| 1. Carga de datos | Lectura del Excel desde Drive | Usar la base original del proyecto |
| 2. Diagnóstico y limpieza | Revisión de nulos, duplicados, infinitos, rangos e IQR | Justificar la calidad de los datos y las decisiones tomadas |
| 3. Análisis exploratorio | Estadísticos, distribuciones, dispersión y correlaciones | Entender las variables antes de modelar |
| 4. Preparación | Separación de variables y codificación de X6 y X8 | Convertir los datos en entradas válidas para los modelos |
| 5. Entrenamiento | Regresión lineal, Random Forest, XGBoost y LightGBM | Comparar métodos distintos |
| 6. Validación | División 80 % / 20 % y validación cruzada de 5 pliegues en entrenamiento | Reducir el riesgo de evaluar el modelo sobre datos que ya conocía |
| 7. Optimización | Optuna: 10 ensayos por objetivo, con 3 pliegues en la búsqueda | Mejorar XGBoost sin consultar el conjunto de prueba |
| 8. Evaluación final | Predicciones sobre el 20 % reservado | Medir desempeño sobre registros no usados para ajustar el modelo |
| 9. Interpretación | Gráficos, análisis de errores e importancia de variables | Comprender resultados y límites del método |
| 10. Conclusiones | Síntesis basada en métricas calculadas | Justificar la elección del modelo |

### ¿Qué significa la limpieza de datos en este trabajo?

La limpieza no consiste en eliminar registros por rutina. Primero se comprueba si existen problemas reales. En la base revisada se encontraron **768 registros válidos, 0 valores faltantes y 0 duplicados exactos**; por eso se conservaron los datos. El notebook también documenta los controles de validez y marca posibles valores atípicos mediante IQR, sin descartarlos automáticamente porque pueden representar combinaciones válidas en una simulación.

### ¿Cómo se elige el mejor modelo?

Primero se comparan los errores promedio durante la **validación cruzada del conjunto de entrenamiento**, utilizando como criterio principal el **RMSE más bajo**. Después de seleccionar el modelo, se calcula su desempeño final con el 20 % que se mantuvo reservado. El conjunto de prueba **no se emplea para escoger hiperparámetros**.

- **MAE:** error absoluto promedio; menor es mejor.
- **RMSE:** error que penaliza más los errores grandes; menor es mejor.
- **R²:** proporción de variación explicada dentro del conjunto evaluado; generalmente, mayor es mejor.

## 6. Resultados obtenidos en la ejecución de referencia

En la ejecución validada con el Excel original, **XGBoost optimizado** fue el modelo seleccionado para ambas variables.

| Variable objetivo | Modelo seleccionado | MAE en prueba | RMSE en prueba | R² en prueba |
|---|---|---:|---:|---:|
| Y1 — Calefacción | XGBoost optimizado | 0.2667 | 0.3646 | 0.9987 |
| Y2 — Refrigeración | XGBoost optimizado | 0.7317 | 1.2846 | 0.9822 |

Estos resultados son **de referencia**, no valores escritos a mano dentro del análisis: se obtienen al ejecutar el notebook. Pueden existir pequeñas diferencias si cambian las versiones de las bibliotecas o el entorno. Para la entrega, se deben revisar las métricas que produzca la ejecución actual.

## 7. Problemas frecuentes y soluciones

| Situación | Qué revisar |
|---|---|
| **Error 403 / permiso denegado** | Iniciar sesión con la cuenta autorizada para abrir el Excel y solicitar permiso de lectura al propietario si fuera necesario |
| **Error 404 / archivo no encontrado** | Confirmar que el Excel sigue existiendo y que el ID usado es el mismo del archivo original |
| **No se encuentra una biblioteca** | Ejecutar primero la celda de instalación y luego volver a iniciar la ejecución desde arriba |
| **Una celda usa una variable no definida** | Ejecutar las celdas en orden mediante **Entorno de ejecución → Ejecutar todas** |
| **Resultados ligeramente diferentes** | Verificar semilla aleatoria (`42`), versión de bibliotecas y que se haya utilizado el mismo dataset |
| **Demora durante la optimización** | Dejar completar los 10 ensayos de Optuna por cada objetivo; no es un ciclo infinito |
| **No aparecen los gráficos** | Verificar que la ejecución haya llegado a las secciones de análisis exploratorio y evaluación |

## 8. Cómo compartir o presentar el proyecto en GitHub

Se recomienda conservar en el repositorio:

- Este `README.md` en la carpeta raíz.
- El notebook `.ipynb` completo.
- Un historial de cambios claro que indique qué se agregó o corrigió.

Para un proyecto grupal, cada integrante puede clonar el repositorio o descargar el notebook, pero necesita **su propio permiso de lectura al archivo de datos** en Google Drive.

Antes de compartir, verificar que el notebook **no contenga contraseñas, tokens ni credenciales**. La autenticación se realiza en la sesión personal de Google Colab y no debe copiarse al repositorio. Se debe respetar el alcance de acceso y las condiciones de uso del dataset.

**Entrega académica:** subir los archivos o enlaces exactamente en el formato que solicite la actividad de la Unidad II en el campus virtual. Este README facilita la reproducción del análisis, pero no reemplaza una instrucción específica de la docente.

## 9. Limitaciones y criterios de integridad académica

1. **Simulación:** los datos corresponden a edificios simulados; extrapolar a edificaciones reales requiere validación adicional.
2. **Predicción y causalidad:** un valor elevado de R² no demuestra que una variable arquitectónica cause por sí sola una variación energética.
3. **Interpretación de resultados:** los mejores parámetros y métricas corresponden al procedimiento y al conjunto de datos documentados.
4. **Reproducibilidad:** el archivo de Drive debe seguir disponible para todas las personas que ejecuten el notebook.
5. **Uso responsable de IA:** ChatGPT apoyó la estructuración de la documentación, los comentarios y la revisión del código; los integrantes deben revisar y comprender el trabajo antes de presentarlo.

## 10. Referencias

- Tsanas, A., & Xifara, A. (2012). *Accurate quantitative estimation of energy performance of residential buildings using statistical machine learning tools*. Energy and Buildings, 49, 560–567.
- UCI Machine Learning Repository. *Energy Efficiency*. DOI: [10.24432/C51307](https://doi.org/10.24432/C51307).
- [Documentación oficial de Google Colab](https://colab.research.google.com/).
- [Documentación de scikit-learn](https://scikit-learn.org/stable/).
- [Documentación de XGBoost](https://xgboost.readthedocs.io/).
- [Documentación de LightGBM](https://lightgbm.readthedocs.io/).
- [Documentación de Optuna](https://optuna.readthedocs.io/).

---

**Antes de entregar:** completar los nombres del equipo, volver a ejecutar el notebook desde cero, revisar los resultados finales y confirmar los requisitos exactos de entrega en el campus virtual.
