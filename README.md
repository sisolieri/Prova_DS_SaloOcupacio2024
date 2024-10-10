# Prueba Técnica: Predicción de Subvenciones Otorgadas

## 1. Introducción
En esta sección se presenta el conjunto de datos con el que se ha trabajado. Este dataset incluye información detallada sobre las subvenciones públicas otorgadas por el Ayuntamiento de Barcelona, con sus respectivas áreas de interés e importes concedidos. Las variables seleccionadas son:

- **Unique_ID**: Identificador único para cada subvención.
- **Import_Total_Atorgat**: El importe total concedido hasta la fecha de predicción.
- **Import_Total_Sollicitat**: El importe total solicitado para la subvención.
- **Àrea_d'Interès**: El área de interés o sector que recibe la subvención.
- **Data**: Fecha correspondiente a cada subvención.

El propósito del proyecto es hacer una predicción del importe total que será otorgado en subvenciones durante septiembre de 2024, utilizando técnicas de **machine learning** y modelos de series temporales.

## 2. Depuración de datos
Para garantizar la calidad del dataset, se aplicaron varias técnicas de preprocesamiento de datos, incluyendo:

- **Tratamiento de valores nulos**: Se eliminaron los registros con valores nulos en campos cruciales como `Tipologia_De_Subvencio`.
- **Transformación Box-Cox**: Para reducir la asimetría de las variables con distribuciones sesgadas, se aplicó la transformación de Box-Cox a `Import_Total_Atorgat` y otras variables relacionadas.
- **Diferenciación**: Para convertir la serie temporal en estacionaria y eliminar tendencias a largo plazo, se aplicó la diferenciación a las variables de la serie temporal.
- **Normalización**: Las variables numéricas se escalaron utilizando un `RobustScaler` para mejorar el rendimiento de los modelos predictivos.

Durante el preprocesamiento, también se filtraron los valores extremos, especialmente para las subvenciones con importes muy elevados, ya que estaban afectando negativamente las métricas de evaluación.

## 3. Resultados
Se probaron varios modelos de predicción, incluyendo:

- **XGBoost**: Este modelo se ajustó utilizando una hiperparametrización con GridSearchCV, y se aplicó la transformación inversa de Box-Cox para obtener los valores predichos.
- **ARIMA**: Utilizado como modelo base para la comparación de resultados.

Las métricas utilizadas para evaluar los modelos incluyen:

- **RMSE** (Root Mean Squared Error): Para medir el error absoluto en la predicción de los importes.
- **MAPE** (Mean Absolute Percentage Error): Para calcular el error porcentual, excluyendo los valores cercanos a cero para evitar distorsiones.

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
