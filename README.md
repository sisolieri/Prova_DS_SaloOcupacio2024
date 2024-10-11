# Subvenciones Otorgadas por el Ayuntamiento de Barcelona: Analisis y Predicción

## 1. Introducción
El dataset de partida para este proyecto incluye información detallada sobre las subvenciones públicas otorgadas por el Ayuntamiento de Barcelona, para las que se ha realizado pagos del año 2019 y siguientes.

A partir de la información inicial disponible, se obtuvieron las siguientes variables, las cuales se utilizaron posteriormente para extraer insights clave y generar características adicionales durante la fase de modelado.

### Descripción de las variables utilizadas

- **Entitat_Municipal**: Organización municipal responsable de otorgar la subvención.
- **Tipologia_De_Subvencio**: Tipo de subvención otorgada (clasificación de las subvenciones).
- **Data_Atorgament**: Fecha en la que se concedió la subvención.
- **Import_Sollicitat**: Importe solicitado para el proyecto subvencionado.
- **Import_Total_Projecte**: Importe total del proyecto presentado para la subvención.
- **Import_Atorgat_Inicial**: Importe inicial concedido como subvención.
- **Àrea_d'Interès**: Área temática o sector de interés al que se destina la subvención.
- **Tipologia_Beneficiari**: Tipo de beneficiario que recibe la subvención (por ejemplo, individual, organización).
- **Bucket_Import_Atorgat**: Clasificación por rangos del importe concedido.


El propósito del proyecto es extraer información útil sobre la distribución de subvenciones y proporcionar al Ayuntamiento una herramienta de previsión que pueda ayudar la planificación financiera y la asignación de recursos.

## 2. Depuración de datos y criterios de evaluación del modelo
### Limpieza profunda de las variables de interés
Dado el número limitado de columnas, las variables más importantes se analizaron individualmente, para resolver errores específicos o agrupar valores únicos de baja frecuencia.
### Tratamiento de valores nulos

Durante la fase de limpieza de datos, se adoptaron las siguientes decisiones para tratar los valores nulos, priorizando la calidad del dataset y evitando la imputación de valores que pudiera introducir errores:

- **Eliminación de registros con baja frecuencia de nulos**: Los registros afectados fueron eliminados, ya que representaban un impacto mínimo en el análisis, afectando menos del 1% del importe total de las subvenciones.

- **Eliminación de la columna `Data_Convocatoria`**: Dado que más del 45% de los registros en esta columna contenían valores nulos, se decidió eliminarla por completo.
  
- **Eliminación de registros con `Organ_Gestor` nulo**: Aunque los registros con valores nulos en esta columna representaban aproximadamente el 11% del total, su distribución a lo largo de los 5 años del dataset (2019-2023) era uniforme. La pérdida de datos resultante afectaba solo al 12% del importe total, y, en promedio, el 2,5% del importe anual, lo cual se consideró un compromiso aceptable.

### Preprocesamiento y métricas utilizadas

- **Logaritmización y diferenciación**: Las variables relacionadas con los importes de las subvenciones (`Import_Total_Atorgat`, `Import_Total_Sollicitat` y `Import_Total_Projectes`) fueron logaritmizadas y posteriormente diferenciadas. Esto ayudó a corregir la fuerte distribución skewed de los datos, ya que en la mayoría de las áreas de interés y clases de importe, la concesión de subvenciones se concentra en periodos limitados del año.
- **Encoding**: Para los modelos multivariantes como **CatBoost** y **XGBoost**, se emplearon diferentes técnicas de encoding dependiendo del tipo de variable:
  - **Ordinal encoding**: Aplicado a las variables categóricas con un orden intrínseco.
  - **One-hot encoding**: Utilizado para las variables de tipo `object` sin un orden inherente.

- **Escalado de los datos**: Tras el encoding, se escaló el dataset utilizando **RobustScaler**, que permite reducir el impacto de los valores extremos durante el ajuste.

- **Métricas utilizadas** para evaluar los modelos:

  - **RMSE** (Root Mean Squared Error): Para medir el error absoluto en la predicción de los importes.
  - **MAPE** (Mean Absolute Percentage Error): Para calcular el error porcentual y útil para mejorar la evaluación objetiva de la bondad del modelo y contextualizar mejor el alto valor del RMSE, debido a los valores muy elevados de los importes acumulados de las subvenciones.


## 3. Resultados
Se probaron varios modelos de predicción, incluyendo:

- **XGBoost**: Este modelo se ajustó utilizando una hiperparametrización con GridSearchCV, y se aplicó la transformación inversa de Box-Cox para obtener los valores predichos.
- **ARIMA**: Utilizado como modelo base para la comparación de resultados.



Los resultados mostraron que el modelo **XGBoost** tuvo un RMSE significativamente menor comparado con **ARIMA**, aunque se detectaron valores con un MAPE elevado para algunos importes superiores a 500.000.

## 4. Conclusiones
Las principales conclusiones derivadas de este proyecto son:

- La transformación de Box-Cox, combinada con la diferenciación, mejoró significativamente la estabilidad de las predicciones para subvenciones de menor importe.
- Se detectó que los valores de subvenciones con importes muy elevados (a partir de 500.000) presentan más dificultades en la predicción, aumentando el MAPE.
- Para futuras mejoras, se recomienda explorar la segmentación de los modelos según el tamaño de las subvenciones para reducir el sesgo causado por los valores extremos.

Los resultados finales pueden servir de base para optimizar la distribución de subvenciones y mejorar la planificación financiera.


La transformación de Box-Cox, combinada con la diferenciación, mejoró significativamente la estabilidad de las predicciones para subvenciones de menor importe.
Se detectó que los valores de subvenciones con importes muy elevados (a partir de 500.000) presentan más dificultades en la predicción, aumentando el MAPE.
Para futuras mejoras, se recomienda explorar la segmentación de los modelos según el tamaño de las subvenciones para reducir el sesgo causado por los valores extremos.
Los resultados finales pueden servir de base para optimizar la distribución de subvenciones y mejorar la planificación financiera.
