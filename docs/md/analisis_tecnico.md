# Proyecto Ventas — Nivel 2: Análisis de Ventas con SQL + Python

## Fase 1 — Auditoría de calidad con SQL

### **Reto 1.1** — Identificación de ventas con `cantidad` negativa o igual a cero.

### La consulta permitió identificar que existen 6 registros de ventas con una cantidad menor o igual a cero, los cuales representan datos inválidos que deberían revisarse o corregirse para garantizar la calidad de la información.
---
### **Reto 1.2** — Detección de ventas con `cliente_id` inexistente en la tabla `clientes` (subconsulta con `NOT IN`) e identificación de huérfanos de integridad referencial.

### La consulta encontró 5 registros huérfanos de integridad referencial en la tabla de ventas, donde dichos registros cuentan con el id 999, mismo que no existe en la tabla clientes.
---
### **Reto 1.3** — Análisis de registros con `descuento_pct` nulo y criterio de tratamiento aplicado.

### La consulta para descuentos nulos mostró un total de 10 registros sin un valor de descuento. Por otro lado, los descuentos con valores iguales a 0 registraron un total de 197 registros, representando aproximadamente la mitad del conjunto de datos. Se eliminaron los 10 registros con `descuento_pct` nulo por representar menos del 3 % del dataset y no presentar un patrón identificable de ausencia.
---
### **Reto 1.4** — Detección de registros duplicados exactos en la tabla `ventas`.

### Para identificar registros duplicados exactos en la tabla ventas, se utilizó el método `duplicated()` de Pandas, que compara todas las columnas del DataFrame y marca como duplicadas aquellas filas que ya tienen un registro idéntico previo. El resultado mostró 3 registros duplicados exactos, lo que indica la existencia de filas repetidas en la base de datos. Para eliminar estos duplicados y conservar únicamente la primera aparición de cada registro, se empleó el método `drop_duplicates()`: `df_ventas = df_ventas.drop_duplicates()`. Finalmente, se verificó que los duplicados fueron eliminados correctamente mediante: `df_ventas.duplicated().sum()`, obteniendo como resultado 0, lo que confirma que el DataFrame ya no contiene registros duplicados exactos.

---

## Fase 2 — Limpieza

### Con base en el análisis inicial de la base de datos y el posterior proceso de limpieza, se realizaron las siguientes acciones:
### Para los registros con cantidad negativa se aplicó un proceso de ETL utilizando Pandas. Los datos fueron extraídos a un DataFrame denominado `df_ventas`, donde se filtraron únicamente las cantidades mayores a cero y posteriormente se cargaron nuevamente en la base de datos.
### El mismo proceso de ETL se aplicó para eliminar los registros huérfanos de integridad referencial, así como los registros duplicados del mismo conjunto de datos y la normalización de los valores correspondientes al campo `segmento` en la tabla clientes.
### Finalmente, los registros con valores nulos en `descuento_pct` fueron eliminados.
---

## Fase 3 — Análisis con JOINs

### **Reto 3.1** — Cálculo del ingreso total por categoría de producto (JOIN ventas + productos).

### El ingreso total por cada categoría dentro del conjunto de datos se distribuye de la siguiente manera:
### TABLA DE VALORES:
### ![alt text](/docs/imgs/ingresoPorCategoria.png)
---
### **Reto 3.2** — Identificación de la ciudad con mayor generación de ingresos (JOIN ventas + clientes + productos).

### La ciudad con mayores ingresos registrados es Barranquilla, con un total de 75.650.200 en ingresos.
### TABLA DE VALORES:
### ![alt text](/docs/imgs/ciudadConMayorIngresos.png)
---
### **Reto 3.3** — Cálculo del ticket promedio (ingreso promedio por venta) según el segmento de cliente.

### El ticket promedio para cada segmento es el siguiente:
### ![alt text](/docs/imgs/ticketPromedio.png)
---
### **Reto 3.4** — Identificación de productos cuyo ingreso total supera el promedio de ingresos de todos los productos mediante una subconsulta.

### La siguiente tabla muestra los productos cuyo ingreso total supera el promedio de ingresos calculado entre todos los productos:
### ![alt text](/docs/imgs/ingresoTotalPromedio.png)
---

## Fase 4 — Visualización

Con los DataFrames que se obtuvieron en la Fase 3, se construye:

```
1. Gráfico de barras — ingreso total por categoría
2. Gráfico de barras — ingreso total por ciudad
3. Boxplot o barras — ticket promedio por segmento
```