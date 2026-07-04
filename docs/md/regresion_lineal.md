# Proyecto Ventas — Regresion Lineal con Scikit-Learn

## Fase 1 — Extracción y preparación de datos

Conecta a `ventas.db` usando la ruta relativa `../data/raw/ventas.db` y define la función `sql()`.

Escribe una query SQL con JOIN entre `ventas` y `productos` que extraiga `cantidad`, `descuento_pct`, `precio_unitario` y `categoria`. Calcula el `ingreso` directamente en la query o en Pandas — ambas opciones son válidas.

Verifica el shape del DataFrame resultante y confirma que no hay nulos antes de continuar.

---
## Fase 2 — Exploración antes de modelar

Antes de entrenar cualquier modelo, siempre se exploran las relaciones entre variables. Esto te dice si la regresión lineal tiene sentido para estos datos y qué variables probablemente van a aportar más.

Calcula la correlación de `cantidad`, `descuento_pct` y `precio_unitario` con `ingreso` usando `.corr()` de forma selectiva — selecciona solo esas 4 columnas, aplica `.corr()` y extrae la columna `"ingreso"`.

### **Punto de reflexión 2.1** — ¿Qué variable tiene mayor correlación con `ingreso`? ¿El resultado tiene sentido desde el punto de vista del negocio, o te sorprende?

### La variable con mayor correlacion con "ingreso" es "precio_unitario" mostrando un resultado de 0.753 seguido de cantidad con un resultado de 0.600, teniendo estas sentido ya que el ingreso de una venta depende directamente de la cantidad vendida de un producto y el precio del mismo
---
### **Punto de reflexión 2.2** — Construye un scatter de `precio_unitario` vs `ingreso`. ¿La relación parece lineal? ¿Hay outliers visibles que podrían afectar el modelo?

### Con base al scatter realizado se puede ver una relacion lineal por parte de los valores en "precio_unitario" e "ingreso" sin presentar outliers evidentes o significativos que puedan afectar el proceso de analisis posterior a este grafico
![alt text](/docs/imgs/regresionLineal/scatter_precioUnitario_Ingresos.png)

---
## Fase 3 — Preparación de features

Scikit-Learn solo acepta valores numéricos. La columna `categoria` es texto, así que debes convertirla con One-Hot Encoding antes de poder incluirla en el modelo.

Aplica `pd.get_dummies()` sobre el DataFrame con `columns=["categoria"]` y `drop_first=True`. Guarda el resultado en un nuevo DataFrame llamado `df_encoded`.

### **Punto de reflexión 3.1** — ¿Cuántas columnas tiene `df_encoded` después del encoding? ¿Por qué `drop_first=True` elimina una columna y qué problema evita?

### El dataframe llamado df_encoded paso a tener un total de 8 columnas al aplicar One-Hot Encoding y al aplicar la funcion drop_first=True durante el proceso se evita la multicolinealidad y evitar posibles fallos con el modelo

---
## Fase 4 — Separar X e y y aplicar train/test split

Separa el DataFrame en:
- `X` — todas las columnas excepto `ingreso`
- `y` — únicamente la columna `ingreso`

Luego aplica `train_test_split` con `test_size=0.2` y `random_state=42`.

### **Punto de reflexión 4.1** — ¿Cuántas filas quedan en train y en test? ¿Es un tamaño razonable para entrenar un modelo de regresión con este dataset?

### Quedan 303 filas en el conjunto de entrenamiento del modelo y 76 filas para el conjunto de prueba del mismo. Considero que es un tamaño razonable para entrenar un modelo de regresion, ya que hay suficientes datos para que el modelo aprenda y un conjunto de prueba adecuado para evaluar su rendimiento

---
## Fase 5 — Entrenamiento del modelo

Crea un modelo `LinearRegression()`, entrénalo con `X_train` e `y_train` usando `.fit()`, y genera predicciones sobre `X_test` con `.predict()`.

Después del entrenamiento, imprime los coeficientes del modelo junto al nombre de cada variable. El coeficiente te dice cuánto sube el ingreso predicho por cada unidad que sube esa variable, manteniendo el resto constante.

### **Punto de reflexión 5.1** — Antes de ver las métricas, mira los coeficientes. ¿La variable con mayor coeficiente coincide con la que tenía mayor correlación en la Fase 2?

### La variable resultante con mayor coeficiente despues de entrenar el modelo de prediccion es cantidad, la cual se menciono en la segunda fase que era la segunda con mayor correlacion con "ingresos", teniendo un resultado de 0.600 en correlacion, y en coeficiente 202359.555177

---
## Fase 6 — Evaluación del modelo

**Antes de ejecutar esta fase**, escribe en una celda markdown tu predicción del R² esperado y justifícala con base en las correlaciones que viste en la Fase 2. Este hábito de predecir antes de ver el resultado es clave en análisis de datos.

Calcula R², MAE y RMSE usando `y_test` e `y_pred`. Para dar contexto al MAE, calcula también qué porcentaje representa del ticket promedio del dataset.

### **Punto de reflexión 6.1** — ¿El R² obtenido era el que esperabas? ¿El MAE es aceptable en el contexto del ticket promedio?

### El R² obtenido fue superior al esperado. Inicialmente se estimo un valor cercano a 0.70, pero el modelo alcanzo un R² de 0.8386, lo que indica que explica aproximadamente el 83.9% de la variabilidad del importe neto. Esto sugiere que el modelo tiene un buen poder predictivo. En cuanto al MAE, fue de 192,621.70, mientras que el ticket promedio es de 842,265.61. Esto significa que el error absoluto promedio representa cerca del 23% del ticket promedio. Aunque existe un margen de error considerable, el desempeño del modelo puede considerarse aceptable para una primera aproximación, ya que las predicciones se mantienen relativamente cercanas al valor real.

### **Punto de reflexión 6.2** — Si el R² es bajo (por ejemplo 0.30), hay dos posibles causas: (A) el modelo lineal no es adecuado para estos datos, o (B) las variables que elegiste no explican bien el ingreso. ¿Cómo distinguirías entre las dos? ¿Qué harías diferente?

### Dado que en el conjunto se datos se trabajo con todas las varibles, podria sugerir investigar sobre otro modelo que tenga mejor funcionamiento con los tipos de datos vistos e implementarlo al flujo de trabajo realizado de regresion 

---
## Fase 7 — Visualización

Construye dos gráficos en una sola figura:

1. **Scatter de predichos vs reales** — los puntos deben concentrarse cerca de la línea diagonal y=x si el modelo ajusta bien. Agrega esa línea diagonal en rojo punteado como referencia.
![alt text](/docs/imgs/regresionLineal/scatter_predichos_reales.png)
2. **Histograma de residuos** — los errores (real − predicho) deben distribuirse aproximadamente centrados en cero. Una distribución muy asimétrica indica que el modelo tiene un sesgo sistemático.
![alt text](/docs/imgs/regresionLineal/histogramaResiduos.png)