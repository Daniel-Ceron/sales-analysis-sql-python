# 📊 Proyecto de Análisis de Ventas con SQL y Python

## 📌 Descripción

Este proyecto simula el trabajo de un Analista de Datos Junior dentro de una empresa comercial. El objetivo es realizar una auditoría de calidad de datos, ejecutar un proceso de limpieza (ETL), analizar la información mediante SQL y Python, generar visualizaciones y presentar conclusiones orientadas al negocio.

Todo el proyecto fue desarrollado siguiendo una metodología similar a la utilizada en entornos empresariales, donde los datos iniciales contienen errores de calidad que deben resolverse antes de realizar cualquier análisis.

---

# 🎯 Objetivos

- Detectar problemas de calidad en los datos.
- Aplicar un proceso de limpieza y transformación (ETL).
- Analizar información mediante consultas SQL.
- Construir visualizaciones para facilitar la toma de decisiones.
- Elaborar recomendaciones basadas en evidencia.

---

# 🛠 Tecnologías utilizadas

- Python
- Pandas
- SQLite
- SQL
- Matplotlib
- Scikit-Learn
- Seaborn
- Jupyter Notebook
- VS Code

---

# 📂 Estructura del proyecto

```
Proyecto-Ventas/

│
├── data/
│   ├── raw/
│   └── processed/
│
├── docs/
│   ├── imgs/
│   └── md/
│
├── notebooks/
│
├── reportes/
│
└── README.md
```

---

# 🔄 Flujo de trabajo

El proyecto se desarrolló siguiendo las siguientes etapas:

```
Auditoría de datos

↓

Limpieza de datos (ETL)

↓

Análisis mediante SQL

↓

Visualización

↓

Conclusiones de negocio
```

---

# 📋 Fases del proyecto

## 🔎 Fase 1 — Auditoría de calidad

Durante esta etapa se identificaron problemas como:

- Cantidades negativas.
- Registros huérfanos.
- Valores nulos.
- Registros duplicados.
- Inconsistencias en variables categóricas.

---

## 🧹 Fase 2 — Limpieza de datos

Se aplicaron procesos de limpieza para mejorar la calidad del conjunto de datos:

- Eliminación de registros inválidos.
- Eliminación de duplicados.
- Corrección de integridad referencial.
- Normalización de categorías.
- Tratamiento de valores nulos.

---

## 📈 Fase 3 — Análisis

Mediante consultas SQL y Pandas se respondió a preguntas de negocio como:

- ¿Qué categoría genera mayores ingresos?
- ¿Qué ciudad produce mayores ventas?
- ¿Cuál es el ticket promedio por segmento?
- ¿Qué productos superan el ingreso promedio?

---

## 📊 Fase 4 — Visualización

Se construyeron gráficos para comunicar los resultados del análisis:

- Barras por categoría.
- Barras por ciudad.
- Ticket promedio por segmento.

---

## 💼 Fase 5 — Conclusiones de negocio

### Hallazgos principales

- **Electrónica** es la categoría con mayor ingreso total (128.3M, 40.2% del total),
  seguida de cerca por **Belleza** (111.7M, 35.0%). Juntas concentran el 75% del 
  ingreso total del período analizado. **Deportes** es la categoría con menor ingreso 
  (13.5M, 4.2%) — no se recomienda descontinuarla basándose únicamente en este dato, 
  ya que el análisis no contempla margen de ganancia, frecuencia de compra ni 
  estacionalidad.

- **Barranquilla** lidera en ingresos con 73.2M (22.9%), pero las primeras cuatro 
  ciudades están agrupadas entre 20% y 23% — no hay una ciudad dominante clara. 
  Antes de priorizar una campaña de marketing en Barranquilla se recomienda cruzar 
  este dato con el número de clientes activos por ciudad para determinar si el 
  liderazgo responde a mayor base de clientes o a un ticket promedio más alto.

- El segmento **Estándar** registró el ticket promedio más alto (901.342 COP), 
  por encima del segmento **Premium** (781.360 COP). Este hallazgo es contraintuitivo 
  y debe interpretarse con cautela dado el tamaño del dataset — se recomienda 
  validarlo con datos reales antes de tomar decisiones sobre la estrategia de 
  segmentación.

---

## 🤖 Fase 6 — Modelo de Regresión Lineal

Se implementó un modelo de regresión lineal con Scikit-Learn para predecir 
el ingreso por venta a partir de variables conocidas al momento de la transacción.

**Variables predictoras:** cantidad, descuento_pct, precio_unitario, categoria (One-Hot Encoding)  
**Variable objetivo:** ingreso = cantidad × precio_unitario × (1 − descuento_pct / 100)

### Resultados del modelo

- **R²: 0.8386** — el modelo explica el 83.9% de la variación en el ingreso por venta
- **MAE: 192.621 COP** — error promedio por predicción (22.87% del ticket promedio)
- **Variable de mayor impacto:** cantidad (coeficiente: 202.359,55)
- **Categoría con menor aporte predicho:** Ropa (coeficiente: −47.827)

### Conclusión del modelo

El modelo supera el umbral mínimo de R² = 0.80 considerado aceptable para producción. 
El modelo no contempla margen de ganancia, estacionalidad ni 
frecuencia de compra.

### Vista previa

![Predichos vs Reales](docs/imgs/regresionLineal/scatter_predichos_reales.png)
![Residuos](docs/imgs/regresionLineal/histogramaResiduos.png)

---

---

## 🔍 Fase 7 — Modelo de Clasificación: Predicción de Clientes Recurrentes

Se construyó un dataset agregado por cliente a partir de la tabla de ventas, 
definiendo como cliente recurrente aquel con 6 o más compras en el período analizado.
Se entrenaron y compararon tres modelos de clasificación.

**Variable objetivo:** cliente_recurrente → 1 = recurrente (≥ 6 compras), 0 = no recurrente  
**Variables predictoras:** total_compras, ingreso_total, descuento_promedio, cantidad_promedio, num_categorias  
**Distribución de clases:** 39 recurrentes (65%) — 21 no recurrentes (35%)

### Resultados por modelo

| Modelo | Accuracy | Recall clase 1 |
|---|---|---|
| Decision Tree | **100%** | **1.00** |
| KNN | 91.7% | 1.00 |
| Logistic Regression | 91.7% | 1.00 |

### Conclusión del modelo

**Decision Tree** obtuvo el mejor desempeño con accuracy perfecta sobre el test set.
Los tres modelos alcanzaron recall perfecto para la clase 1 — ninguno dejó pasar 
un cliente recurrente sin detectarlo, que es la métrica más relevante para campañas 
de fidelización.

Los resultados deben interpretarse con cautela dado el tamaño reducido del test set 
(12 filas) — se recomienda reentrenar con un dataset más grande antes de usar las 
predicciones en producción.

### Vista previa

![Matrices de confusión](docs/imgs/clasificacion/matricesConfusion.png)
![Accuracy por modelo](docs/imgs/clasificacion/accuracyModelo.png)

---

# 📌 Principales aprendizajes

Este proyecto permitió fortalecer habilidades relacionadas con:

- SQL (JOINs y subconsultas)
- Limpieza de datos
- ETL con Pandas
- Integridad referencial
- Visualización de datos
- Documentación técnica
- Análisis exploratorio
- Pensamiento analítico orientado al negocio
- Regresión lineal con Scikit-Learn
- Evaluación de modelos (R², MAE, RMSE)
- One-Hot Encoding
- Train/Test Split
- Interpretación de coeficientes
- Clasificación con Logistic Regression, KNN y Decision Tree
- Matriz de confusión e interpretación de errores
- Métricas de clasificación (accuracy, precision, recall, F1)
- Comparación de múltiples modelos sobre el mismo dataset
- Dataset agregado por cliente con SQL

---

# 📷 Vista previa

## Ingreso por categoría

![Ingreso por categoría](docs/imgs/analisisTecnico/ingresoPorCategoria.png)

---

## Ciudad con mayores ingresos

![Ciudad](docs/imgs/analisisTecnico/ciudadConMayorIngresos.png)

---

## Ticket promedio

![Ticket](docs/imgs/analisisTecnico/ticketPromedio.png)

---

# 👨‍💻 Autor

**Daniel Eduardo Cerón Castillo**

Tecnólogo en Desarrollo de Software.

Interesado en:

- Análisis de Datos
- Business Intelligence
- Visualización de Datos
- Machine Learning