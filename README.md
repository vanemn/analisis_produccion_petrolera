# Análisis producción petrolera

#  Informe de Tarea Interna – Análisis de Producción Petrolera en Colombia

  
**Departamento:** Dirección Técnica de Producción  
**Área Responsable:** Inteligencia Operativa – Análisis de Datos  
**Archivo Base:** petroleum_colombia_500.csv  
**Fecha:** 4 de julio de 2025

---

## Objetivo del Proyecto

Realizar un análisis integral del comportamiento diario de producción de petróleo, gas y condiciones de yacimiento en cuatro campos colombianos para:

- Comparar la eficiencia productiva entre campos
- Detectar presencia de agua de formación
- Identificar relaciones entre presión, profundidad y producción
- Validar un modelo predictivo de producción en función de la presión

---

## 1. Preparación de los Datos y EDA

- Se creó una clase `eda()` para revisar estructura, tipos, estadísticos descriptivos, valores nulos, outliers e incluso limpieza con IQR.
- Se detectaron y filtraron outliers en variables numéricas clave.
- La columna `date` se convirtió al tipo datetime correctamente.
- Se identificaron 452 registros limpios tras la limpieza (de los 500 originales).

---

## 2. Exploración de Variables

### Producción de Petróleo

Durante la exploracion se denota que la mayoría de los pozos producen entre 30.000 y 70.000 barriles diarios. Se detectaron algunos valores atípicos por encima de 100.000.

![Histograma Oil](fig/histograma_oil.png)  

La gráfica muestra la distribución de la producción de petróleo (oil_bbl):

Eje X (barril de petróleo):
Representa la cantidad de barriles de petróleo producidos. Los valores van desde menos de 40.000 hasta más de 100.000 barriles.

Eje Y :
Representa la frecuencia o el número de veces que se observa una determinada cantidad de producción de petróleo.

Distribución bimodal:
La gráfica sugiere una distribución bimodal, lo que significa que hay dos picos principales o modas en la distribución de la producción de petróleo.
El primer pico se encuentra alrededor de los 40.000 barriles, indicando que hay una alta frecuencia de producción de petróleo en ese rango.
El segundo pico se sitúa cercano a los 90.000 barriles, lo que sugiere otra concentración significativa de producción en ese nivel.

Curva de densidad (línea verde):
La línea verde superpuesta es una estimación de la densidad de probabilidad, que suaviza la distribución y resalta los dos picos mencionados, mostrando la forma general de la distribución de la producción de petróleo.

Rangos de menor frecuencia:
Se observa una menor frecuencia de producción en rangos intermedios, como entre los 50.000 y 70.000 barriles, donde la altura de las barras y la curva de densidad son menores. También hay una disminución en la frecuencia en los extremos de la distribución, tanto por debajo de los 30.000 como por encima de los 100.000 barriles.


### Producción de Gas por Campo

![Boxplot Gas](fig/boxplot_gas.png)  
La dispersión en la producción de gas es significativa. El campo **Cupiagua** muestra gran variabilidad.

Mediana (línea central dentro de la caja):
Rubiales : tiene la mediana más alta, lo que indica que la mitad de la producción de gas de este campo supera a la de los otros.
Cupiagua : y Cusiana tienen las medianas más bajas, sugiriendo una menor producción de gas en promedio.

Rango Intercuartílico (IQR - la caja):
Rubiales : presenta una caja más grande, lo que sugiere una mayor variabilidad en la producción de gas dentro de este campo en comparación con los demás.
Cupiagua : y Cusiana tienen cajas más pequeñas, indicando una menor dispersión en sus datos de producción.

Bigotes (líneas que se extienden desde la caja):
Los bigotes muestran el rango general de los datos, excluyendo los valores atípicos.

Valores Atípicos (puntos fuera de los bigotes):
Se observan valores atípicos (puntos) en Cupiagua, Rubiales, Chichimene y Cusiana, lo que indica mediciones de producción que se desvían significativamente del patrón general de cada campo.

📁 [frecuencia_por_campo.csv](fig/frecuencia_por_campo.csv) confirma una distribución de registros relativamente balanceada entre campos.

---

##  3. Análisis de Agua de Formación

Se generó la variable `alta_agua` (True si corte de agua ≥ 30%) y se analizaron sus frecuencias por campo.

📁 [tabla_alta_agua.csv](fig/tabla_alta_agua.csv)  
📁 [porcentaje_alta_agua.csv](fig/porcentaje_alta_agua.csv)

| Campo       | % Días con Alta Agua |
|-------------|-----------------------|
| Cusiana     | 37.96%                |
| Cupiagua    | 32.00%                |
| Chichimene  | 29.20%                |
| Rubiales    | 27.20%                |

**Conclusión:** Cusiana y Cupiagua muestran signos de envejecimiento o intrusión de agua.

---

## 4. Estadística Básica y Correlación

![Heatmap](fig/heatmap_correlacion.png)

Se evaluaron correlaciones entre variables numéricas.

- Producción (`oil_bbl`) muestra débil correlación positiva con presión del yacimiento.
- Temperatura y profundidad no presentan correlaciones significativas.

---

## 5. Relaciones Visuales

### Presión vs Producción

![Presión vs Producción](fig/Presion_vs_produccion.png)  
Aunque se sugiere una relación directa, hay mucha dispersión. No se observa tendencia clara.

### Pairplot

![Pairplot](fig/pairplot_variables.png)  
Relaciones entre variables clave coloreadas por campo. Destacan patrones distintos entre Rubiales y Cupiagua, especialmente en gas y temperatura.

---

## 6. Regresión Lineal Simple

Se modeló la producción de petróleo (`oil_bbl`) en función de la presión del yacimiento (`reservoir_pressure_psi`).

📁 [metricas_modelo.csv](fig/metricas_modelo.csv)

| Métrica             | Resultado     |
|---------------------|---------------|
| β₁ (coeficiente)    | ≈ 0.44        |
| β₀ (intercepto)     | ≈ 52,339      |
| R²                  | ≈ -0.0397     |
| MSE                 | ≈ 448,165,580 |

 **Conclusión:** La presión no explica bien la variabilidad de producción. El modelo tiene bajo poder predictivo y requiere incorporar más variables.

---

## 7. Conclusiones y Recomendaciones

- **Campos a monitorear:** *Cusiana* y *Cupiagua* debido a mayor proporción de días con alto corte de agua.
- **Modelado:** El modelo lineal simple no es suficiente. Se recomienda un modelo multivariado no lineal.
- **Eficiencia operativa:** Rubiales y Chichimene mantienen valores operativos más estables.

---

 *Análisis desarrollado con Python, pandas, seaborn, scikit-learn y matplotlib*  

