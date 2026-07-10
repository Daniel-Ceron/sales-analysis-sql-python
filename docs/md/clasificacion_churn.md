# Proyecto de Ventas — Clasificación de Clientes Recurrentes

## Fase 1 — Construcción del conjunto de datos

Para el desarrollo del modelo de clasificación se construyó un conjunto de datos agregado por cliente, en el que cada registro representa el comportamiento histórico de un cliente dentro de la empresa.

La información se obtuvo mediante una consulta SQL que integra las tablas `ventas` y `productos` utilizando un `JOIN`. Posteriormente se calcularon las siguientes variables:

- Total de compras realizadas por cliente.
- Ingreso total generado.
- Descuento promedio recibido.
- Cantidad promedio de productos por compra.
- Número de categorías diferentes adquiridas.

A partir de estas variables se creó la variable objetivo **cliente_recurrente**, clasificando como recurrentes aquellos clientes con seis o más compras registradas.

Finalmente, se verificó la distribución de las clases antes del entrenamiento de los modelos.

### Distribución de la variable objetivo

El conjunto de datos quedó conformado por **60 clientes**, de los cuales **39 fueron clasificados como clientes recurrentes** y **21 como no recurrentes**.

Aunque existe una ligera diferencia entre ambas clases, la distribución continúa siendo suficientemente equilibrada para entrenar modelos de clasificación sin presentar un problema severo de desbalance.

---

## Fase 2 — Análisis exploratorio

Antes del entrenamiento de los modelos se realizó un análisis exploratorio con el objetivo de identificar qué variables presentaban mayor capacidad para diferenciar clientes recurrentes y no recurrentes.

Para ello se construyeron diagramas de caja para cada variable predictora y se calculó la matriz de correlación respecto a la variable objetivo.

### Variables con mayor capacidad predictiva

La variable **total_compras** presentó la mayor diferencia entre ambos grupos.

En el diagrama de caja se observa que los clientes recurrentes poseen una mediana considerablemente superior, comportamiento que coincide con la matriz de correlación, donde esta variable obtuvo una correlación positiva de **0.82** respecto a `cliente_recurrente`.

Desde la perspectiva del negocio, este resultado es coherente, ya que un mayor número de compras incrementa la probabilidad de que un cliente sea considerado recurrente.

### Variables con correlación negativa

Las variables **descuento_promedio** (-0.07) y **cantidad_promedio** (-0.02) presentaron correlaciones negativas muy cercanas a cero.

Estos resultados indican que ninguna de estas variables ejerce una influencia significativa sobre la probabilidad de que un cliente sea recurrente dentro del conjunto de datos analizado.

---

## Fase 3 — Preparación de los datos

Una vez construido el conjunto de datos, se realizó la separación entre variables predictoras y variable objetivo.

Posteriormente, el dataset se dividió en conjuntos de entrenamiento y prueba utilizando un **80 % para entrenamiento** y un **20 % para evaluación**.

### Evaluación del tamaño del conjunto de prueba

El conjunto de prueba quedó conformado por **12 registros**, equivalentes al 20 % del total de clientes.

Aunque esta distribución permite validar el funcionamiento de los modelos, el tamaño reducido del dataset implica que una única clasificación incorrecta representa una variación cercana al **8.3 % del accuracy**.

Por esta razón, los resultados obtenidos deben interpretarse con cautela y no como evidencia definitiva del desempeño del modelo.

Para obtener conclusiones más robustas sería recomendable entrenar los modelos con un conjunto de datos considerablemente mayor.

---

## Fase 4 — Entrenamiento de modelos

Se entrenaron tres algoritmos de clasificación utilizando exactamente la misma partición de entrenamiento y prueba para garantizar una comparación objetiva entre ellos.

Los modelos evaluados fueron:

- Logistic Regression
- K-Nearest Neighbors (KNN)
- Decision Tree

En el caso de KNN se realizó previamente el escalamiento de las variables debido a la sensibilidad de este algoritmo frente a diferencias de escala.

---

## Fase 5 — Evaluación de modelos

El desempeño de cada modelo fue evaluado mediante métricas de clasificación como Accuracy, Precision, Recall y F1-Score.

### Comparación del desempeño

El modelo **Decision Tree** obtuvo los mejores resultados, alcanzando valores máximos tanto en **Accuracy** como en **Recall** para la clase de clientes recurrentes.

Sin embargo, este comportamiento no implica que ambas métricas deban coincidir necesariamente, ya que cada una evalúa aspectos diferentes del modelo.

Mientras el Accuracy representa la proporción total de predicciones correctas, el Recall mide específicamente la capacidad para identificar correctamente los clientes recurrentes.

### Métrica de mayor interés para el negocio

Desde la perspectiva comercial, la métrica más relevante es el **Recall**.

Maximizar esta métrica permite identificar la mayor cantidad posible de clientes recurrentes, reduciendo la probabilidad de excluir clientes valiosos de futuras campañas de fidelización.

---

## Fase 6 — Visualización de resultados

Para facilitar la interpretación de los resultados se elaboraron dos visualizaciones principales.

### Matrices de confusión

![alt text](/docs/imgs/clasificacion/matricesConfusion.png)

Las matrices de confusión permiten comparar visualmente el comportamiento de los tres modelos e identificar aciertos y errores de clasificación.

### Comparación de Accuracy

![alt text](/docs/imgs/clasificacion/accuracyModelo.png)

El gráfico resume el desempeño general de cada algoritmo utilizando la métrica Accuracy.

---

## Fase 7 — Conclusiones

### Modelo recomendado

El algoritmo **Decision Tree** presentó el mejor desempeño durante la evaluación, obteniendo los valores más altos de Accuracy y Recall.

Con base en estos resultados, fue seleccionado como el modelo con mejor comportamiento para este conjunto de datos.

### Variables más relevantes

Las variables con mayor capacidad para predecir la recurrencia de un cliente fueron:

- **total_compras (0.82)**
- **num_categorias (0.56)**
- **ingreso_total (0.51)**

Estas variables concentran la mayor información relacionada con el comportamiento histórico de compra de los clientes.

### Limitaciones del modelo

El principal factor limitante corresponde al tamaño del conjunto de datos.

El entrenamiento se realizó únicamente con **60 clientes**, lo cual restringe la capacidad de generalización del modelo y aumenta la probabilidad de sobreajuste.

En consecuencia, los resultados deben interpretarse como una prueba de concepto y no como una solución lista para implementarse en un entorno de producción.

### Recomendación para el negocio

Aunque el modelo puede servir como apoyo para identificar clientes potencialmente no recurrentes, no debería utilizarse como único criterio para tomar decisiones comerciales.

Antes de implementar campañas de retención sería recomendable ampliar el volumen de datos disponible y volver a entrenar los modelos, garantizando así una mayor capacidad de generalización y una reducción del riesgo asociado a decisiones basadas en predicciones incorrectas.