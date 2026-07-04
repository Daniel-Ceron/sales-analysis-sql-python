# 📄 Proyecto Ventas — Modelo de Predicción con Regresión Lineal (Scikit-Learn)

## Fase 1 — Extracción y preparación de datos

### Conexión a la base de datos

Se estableció la conexión con la base de datos `ventas.db` mediante una ruta relativa, utilizando una función auxiliar para ejecutar consultas SQL desde Python.

### Extracción de información

Se realizó una consulta SQL con `JOIN` entre las tablas `ventas` y `productos` para obtener las variables necesarias para el modelo:

- cantidad
- descuento_pct
- precio_unitario
- categoria

A partir de estas variables se calculó el ingreso generado por cada venta.

### Validación inicial

Se verificó la estructura del DataFrame obtenido, confirmando la cantidad de registros, columnas y la ausencia de valores nulos antes de iniciar el proceso de modelado.

---

# Fase 2 — Exploración de variables

## Análisis de correlaciones

Se evaluó la relación entre las variables predictoras y la variable objetivo (`ingreso`) mediante una matriz de correlación.

El análisis mostró que la variable **precio_unitario** presentó la mayor correlación con el ingreso (0.753), seguida por **cantidad** (0.600). Este comportamiento resulta coherente desde el punto de vista del negocio, ya que ambas variables influyen directamente sobre el valor final de una venta.

---

## Relación entre precio unitario e ingreso

Se construyó un gráfico de dispersión para analizar visualmente la relación entre ambas variables.

El comportamiento observado evidencia una tendencia lineal positiva, sin presencia de valores atípicos significativos que puedan afectar el entrenamiento del modelo.

![alt text](/docs/imgs/regresionLineal/scatter_precioUnitario_Ingresos.png)

---

# Fase 3 — Preparación de variables predictoras

## Codificación de variables categóricas

Las variables categóricas fueron transformadas mediante **One-Hot Encoding**, permitiendo que el algoritmo de regresión pudiera utilizarlas correctamente.

Después de la transformación, el conjunto de datos quedó compuesto por ocho variables predictoras.

Se utilizó el parámetro `drop_first=True` con el propósito de evitar la multicolinealidad, eliminando una categoría de referencia durante el proceso de codificación.

---

# Fase 4 — División del conjunto de datos

## Entrenamiento y prueba

El conjunto de datos fue dividido utilizando `train_test_split`, reservando aproximadamente el 80 % de los registros para entrenamiento y el 20 % para evaluación.

La distribución final fue:

- Entrenamiento: 303 registros.
- Prueba: 76 registros.

Esta partición proporciona una cantidad suficiente de información para entrenar el modelo y evaluar posteriormente su capacidad de generalización.

---

# Fase 5 — Entrenamiento del modelo

## Construcción del modelo

Se implementó un modelo de **Regresión Lineal** mediante la clase `LinearRegression()` de Scikit-Learn.

Posteriormente se entrenó utilizando el conjunto de entrenamiento y se generaron predicciones sobre el conjunto de prueba.

## Interpretación de coeficientes

Tras el entrenamiento se analizaron los coeficientes obtenidos por el modelo.

La variable **cantidad** presentó el mayor coeficiente, indicando que es la característica con mayor influencia sobre el ingreso predicho cuando las demás variables permanecen constantes.

Aunque durante el análisis exploratorio la variable con mayor correlación fue **precio_unitario**, el resultado obtenido es consistente, ya que correlación y coeficiente representan conceptos estadísticos diferentes.

---

# Fase 6 — Evaluación del modelo

## Métricas de desempeño

El modelo obtuvo un coeficiente de determinación (**R²**) de **0.8386**, lo que indica que explica aproximadamente el **83.9 %** de la variabilidad presente en los ingresos.

Asimismo, el Error Absoluto Medio (**MAE**) fue de **192,621.70**, mientras que el ticket promedio del negocio corresponde a **842,265.61**.

En términos relativos, el error representa cerca del **23 %** del valor promedio de una venta, por lo que el desempeño del modelo puede considerarse adecuado como una primera aproximación para realizar predicciones.

## Limitaciones del modelo

Aunque el desempeño obtenido fue satisfactorio, este modelo representa únicamente una aproximación lineal al problema.

En escenarios donde el coeficiente de determinación fuera considerablemente menor, sería necesario evaluar si la relación entre las variables no es lineal o si existen variables adicionales que expliquen mejor el comportamiento del ingreso.

---

# Fase 7 — Visualización y validación del modelo

## Comparación entre valores reales y predichos

Se construyó un gráfico de dispersión comparando los valores reales frente a las predicciones realizadas por el modelo.

La proximidad de los puntos respecto a la línea diagonal evidencia un buen nivel de ajuste.

![alt text](/docs/imgs/regresionLineal/scatter_predichos_reales.png)

---

## Distribución de residuos

Finalmente se analizó la distribución de los residuos (error entre el valor real y el valor predicho).

La distribución obtenida se encuentra centrada alrededor de cero y no presenta patrones evidentes de sesgo, lo que respalda el comportamiento esperado para un modelo de regresión lineal.

![alt text](/docs/imgs/regresionLineal/histogramaResiduos.png)