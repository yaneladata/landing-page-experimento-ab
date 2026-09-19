# 🧪 Validación de Hipótesis de Negocio y Experimento A/B | E-commerce Landing Page

> 👤 **Rol:** Analista de Datos en Marketing Digital (Proyecto Individual)  
> 🏢 **Contexto:** Caso de Negocio / Proyecto de Portafolio (Bootcamp Analytics - Proyecto 8)  
> 🎯 **Alcance:** Evaluación de Experimento A/B, Pruebas de Hipótesis Estadísticas ($t$-Student, $z$-Test de Proporciones, Chi-Cuadrado de Independencia), Cuantificación de Impacto Financiero y Recomendaciones de Despliegue.  
> 🛠️ **Stack Técnico:** Python (`pandas`, `numpy`, `scipy.stats`, `statsmodels`, `seaborn`, `matplotlib`), Jupyter Notebook, Estadística Inferencial.

---

## 🎯 Contexto y Desafío de Negocio

El equipo de Marketing Digital de una empresa de e-commerce ejecutó un **experimento A/B** en la página de inicio (*landing page*) durante 28 días (40,000 usuarios expuestos), comparando dos versiones:
* **Página A (Control):** Diseño actual de la landing page.
* **Página B (Tratamiento):** Rediseño propuesto de la landing page.

El objetivo del negocio es determinar, mediante **evidencia estadística sólida e inferencial**, qué versión debe implementarse de forma definitiva para maximizar la **tasa de conversión** y el **valor promedio de compra por usuario**, evaluando además si el canal de tráfico o el tipo de usuario justifican una estrategia de segmentación personal.

---

## 📂 Dataset y Unidades de Análisis

* **Archivo:** `/datasets/landing_experiment.csv` (40,000 registros balanceados: ~20,000 usuarios por variante).
* **Unidad de Análisis:** Cada fila representa un usuario único expuesto a una sola versión de la página.
* **Variables Clave:**
  * `landing`: Grupo del experimento (`A` vs `B`).
  * `converted`: Variable binaria objetivo (`1` = Compra realizada, `0` = No convirtió).
  * `gasto`: Valor económico gastado por el usuario (condicionado a `converted == 1`).
  * `traffic_source`: Canal de adquisición (`Email`, `Ads`, `Organic`, `Referral`).
  * `user_type`: Segmento de cliente (`Nuevo` vs `Recurrente`).
  * `region` y `device`: Variables de entorno y dispositivo.

---

## ⚙️ Metodología y Pruebas Estadísticas Aplicadas

El flujo de análisis en **Jupyter Notebook** incluyó la verificación de supuestos y la aplicación de las siguientes pruebas de hipótesis (nivel de significancia $\alpha = 0.05$):

| Pregunta de Negocio | Variable Evaluada | Prueba Estadística | Muestra / Condición |
| :--- | :--- | :--- | :--- |
| **Gasto Promedio por Comprador** | `gasto` (Numérica Continua) | **Prueba $t$ de Student** (Muestras independientes) | Solo usuarios convertidos (`converted == 1`) |
| **Tasa de Conversión (CR)** | `converted` (Binaria Categorical) | **Prueba $z$ de Proporciones** (Two-sample $z$-test) | Total de usuarios por grupo ($N = 40,000$) |
| **Efecto de Fuente de Tráfico** | `traffic_source` vs `converted` | **Prueba $\chi^2$ de Independencia** | Matriz de contingencia por canales |
| **Efecto del Tipo de Usuario** | `user_type` vs `converted` | **Prueba $\chi^2$ de Independencia** | Usuarios nuevos vs recurrentes |

---

## 💡 Resultados Estadísticos & Hallazgos Clave

### 📊 1. Comparación de Rendimiento (Página A vs. Página B)

* **Gasto Promedio por Usuario Convertido:**
  * **Página A:** $61.09  
  * **Página B:** $68.75 (+ $7.66 a favor de B)  
  * *Resultado Estadístico:* $t = -9.37$, **$p\text{-value} = 1.06 \times 10^{-20}$** ($p < 0.05$).  
  * *Conclusión:* **Se rechaza $H_0$**. Los usuarios que convierten en la Página B gastan significativamente más que los de la Página A.

* **Tasa de Conversión (Conversion Rate):**
  * **Página A:** 12.57% (2,512 / 19,982 usuarios)  
  * **Página B:** 15.96% (3,194 / 20,018 usuarios) (+ 3.38 puntos porcentuales)  
  * *Resultado Estadístico:* $z = -9.68$, **$p\text{-value} = 3.76 \times 10^{-22}$** ($p < 0.05$).  
  * *Conclusión:* **Se rechaza $H_0$**. La Página B convierte una proporción estadísticamente superior de usuarios.

---

### 🔍 2. Análisis de Segmentación (Canal y Tipo de Usuario)

* **Por Fuente de Tráfico (`traffic_source`):**
  * Las tasas oscilan entre el 13.79% (*Organic*) y el 14.99% (*Email*), con una variación máxima de solo **1.2 pp**.
  * *Resultado:* $\chi^2$, **$p\text{-value} = 0.034$** ($p < 0.05$).  
  * *Interpretación:* Aunque existe significancia estadística matemática por el volumen muestral, la diferencia es **irrelevante en la práctica**. No se justifica personalizar la landing por canal.

* **Por Tipo de Usuario (`user_type`):**
  * Usuarios Nuevos (14.36%) vs. Recurrentes (14.09%) (Diferencia de **0.27 pp**).
  * *Resultado:* $\chi^2$, **$p\text{-value} = 0.474$** ($p > 0.05$).  
  * *Interpretación:* **No se rechaza $H_0$**. La conversión es completamente independiente de si el usuario es nuevo o recurrente.

---

## 💰 Impacto Financiero y Recomendaciones Estratégicas

### 🚀 Recomendación Principal: Migración Escalada a la Página B
* **Impacto Estimado:** Se calcula un incremento incremental directo de **~$66,129 USD** sobre la muestra analizada ($682$ compradores adicionales en B $\times \$68.75$ + $2,512$ compradores de A $\times \$7.66$ adicionales).
* **Estrategia de Despliegue:** Se sugiere un **escalamiento gradual** (25% $\rightarrow$ 50% $\rightarrow$ 100% del tráfico) monitoreando métricas clave semana a semana para mitigar el *efecto novedad* antes de consolidar el tráfico total.
* **Optimización de Recurso Técnico:** Se recomienda **desestimar proyectos de personalización por fuente de tráfico o tipo de usuario**, reorientando los esfuerzos de diseño a otras variables no evaluadas (como `region` o `device`).

---

## ⚠️ Limitaciones del Estudio y Próximos Pasos

* **Ventana Temporal Limitada:** El experimento corrió durante 28 días (1 al 28 de enero), período susceptible a sesgos estacionales (post-fiestas de fin de año o estacionalidad de enero).
* **Efecto Novedad (Novelty Effect):** Parte del incremento en la conversión podría estar influenciado por la primera reacción de los usuarios al nuevo diseño visual, tendiendo a estabilizarse con el tiempo.
* **Próximos Pasos:**
  1. Dar seguimiento semanal a las métricas tras el despliegue al 100%.
  2. Evaluar el impacto cruzado en las dimensiones de `region` y `device`.

---

## 🖼️ Visualizaciones del Proyecto

Todas las gráficas fueron generadas en Python (`seaborn` / `matplotlib`) dentro del notebook:
- **Gráfico de Conversión**: Canal de Usuario vs Cantidad de Usuarios
- **Gráfico de Conversión**: Tipo de Usuario vs Cantidad de Usuarios
- **Gráfico Tasa de Conversión**: Canal de Usuario vs Porcentaje
- **Gráfico Tasa de Conversión**: Tipo de Usuario vs Porcentaje
---

## 📁 Estructura del Repositorio

```text
├── visualizaciones/                                 <- Gráficos de pruebas de hipótesis (.png)
│   ├── 01_conversion_rate_comparison.png
│   ├── 02_gasto_promedio_distribution.png
│   └── 03_segmentacion_canales.png
├── data/                                   <- Datasets del experimento
│   └── landing_experiment.csv              <- Datos brutos del A/B test (40k registros)
├── notebook/
│   └── ab_testing_landing_experiment.ipynb <- Notebook reproducible con pruebas inferenciales
└── README.md                               <- Documentación técnica e informe ejecutivo



