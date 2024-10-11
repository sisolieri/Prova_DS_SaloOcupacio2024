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


El propósito del proyecto es extraer información útil sobre la distribución de subvenciones y proporcionar al Ayuntamiento una herramienta de previsión que pueda ayudar la planificación financiera y la asignación de recursos. En concreto, a partir de los datos disponibles hasta agosto de 2024, se desarrolló un modelo capaz de predecir el importe total que se desembolsará en subvenciones durante septiembre de 2024, desglosado por área de interés y por rango de importe concedido.


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

### 3.1 Resultados del análisis estadístico

La **Ciudad de Barcelona** ha invertido más en **Cultura y deporte** y **Otras Áreas de Interés**, especialmente desde 2020, con fluctuaciones en estos sectores. **Economía** y **Distritos** han recibido fondos de manera constante, mientras que las subvenciones a **Urbanismo** y **Derechos sociales** han disminuido en los últimos años. En general, la prioridad reciente parece estar en cultura y áreas de interés más diversas.

Barcelona distribuye las subvenciones más pequeñas principalmente en áreas como **Derechos sociales y personas con discapacidad** y **Economía**, mientras que las más grandes se concentran en **Cultura y deporte** y **Urbanismo**. En cuanto a la tipología, las subvenciones menores suelen ser **ayudas específicas** como la relacionada con el **IBI**, mientras que las de mayor importe se destinan a **transferencias a entes públicos** y proyectos de mayor envergadura.

### 3.2 Resultados del forecasting

Para realizar el forecasting del **Importe Total Ortogado** de las subvenciones, se evaluaron tres modelos: **XGBoost**, **CatBoost** y **ARIMA**. Los resultados de las métricas utilizadas, **RMSE** y **MAPE**, fueron los siguientes:

| Modelo   | RMSE         | MAPE  |
|----------|--------------|-------|
| XGBoost  | 441,278.95   | 7.28  |
| CatBoost | 442,515.47   | 7.15  |
| ARIMA    | 651,207.72   | 7.11  |

De los tres modelos, **XGBoost** presenta el menor **RMSE**, lo que indica que tiene un menor error absoluto en sus predicciones, aunque su **MAPE** es ligeramente mayor que el de los otros modelos. Por otro lado, **ARIMA** tiene el **MAPE** más bajo, lo que significa que, en términos porcentuales, sus predicciones son más precisas. Sin embargo, presenta un **RMSE** más alto, lo que indica una mayor variabilidad en las diferencias absolutas con respecto a los valores reales.

Finalmente, se optó por utilizar **XGBoost** debido a su mejor rendimiento en términos absolutos, lo que lo hace más adecuado para este tipo de previsión financiera. Se ha decidido no realizar una hiperparametrización de los parámetros porque, en este contexto, el modelo actual sirve principalmente como una herramienta de orientación para ayudar al Ayuntamiento en la planificación financiera de las subvenciones. No existe una necesidad crítica de reducir al mínimo el error, ya que en la práctica, el Ayuntamiento siempre tomará un margen de seguridad respecto a las previsiones generadas. Esto significa que los resultados actuales son suficientemente precisos para el propósito de planificación, sin requerir una mejora adicional en las métricas a través de ajustes finos.


## 4. Conclusiones
El análisis de los datos de subvenciones concedidas por la **Ciudad de Barcelona** nos ha permitido identificar patrones claros de inversión en distintas áreas de interés a lo largo de los años. Se ha observado que áreas como **Cultura y deporte** han sido prioritarias desde 2020, con un enfoque continuo en sectores culturales y de interés más amplio. Esta tendencia refuerza el compromiso de la ciudad con la promoción de actividades culturales y recreativas, así como con proyectos más diversos.

En cuanto a la predicción del **Importe Total Atorgado**, tras evaluar los modelos **XGBoost**, **CatBoost** y **ARIMA**, se eligió **XGBoost** por ofrecer el menor **RMSE** (441,278), lo que garantiza una mayor precisión absoluta. Aunque el **MAPE** de **XGBoost** es del **7.28%**, este margen de error es aceptable para la planificación financiera del **Ayuntamiento**, que siempre se reserva un margen de seguridad en sus previsiones.

La principal inferencia derivada de este análisis es que los **modelos predictivos** pueden ser una herramienta útil para la **planificación estratégica**. En particular, el modelo permite al **Ayuntamiento** anticipar las necesidades de financiamiento mes a mes y ajustar su distribución de recursos con mayor precisión, especialmente en áreas de interés prioritarias. A largo plazo, este tipo de análisis puede ayudar a mejorar la eficiencia en la asignación de fondos públicos, permitiendo optimizar los recursos disponibles en función de las necesidades reales de los distintos sectores.

Además, la diferenciación entre **proyectos pequeños y grandes** es clave para entender cómo el **Ayuntamiento** podría ajustar su enfoque en el futuro. Los **proyectos más grandes** tienden a recibir un importe más acorde con sus necesidades, mientras que los **proyectos pequeños** suelen tener una mayor discrepancia entre el importe solicitado y el concedido. Este es un aspecto a considerar en la mejora de futuras políticas de subvenciones.

En resumen, el análisis no solo proporciona un **modelo de previsión útil** para la planificación, sino que también sugiere áreas clave donde el **Ayuntamiento de Barcelona** puede refinar y mejorar la distribución de subvenciones en el futuro, garantizando una mayor equidad y eficiencia en el proceso.
