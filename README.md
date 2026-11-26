# Modelo de Predicción de Riesgo de Crédito

Proyecto de análisis de datos y aprendizaje automático enfocado en riesgo crediticio sobre el dataset Give Me Some Credit. El objetivo es estimar la probabilidad de impago, optimizar la toma de decisiones financieras mediante costos asimétricos y garantizar la explicabilidad de cada predicción con SHAP.


## Objetivos

El objetivo principal del proyecto es maximizar la rentabilidad de la cartera crediticia, minimizando las pérdidas derivadas del incumplimiento de pago y manteniendo un nivel de colocaciones que contribuya al crecimiento sostenible del negocio.

Desde el enfoque de Machine Learning, se plantea desarrollar un modelo de clasificación predictiva capaz de estimar el riesgo de impago de los clientes. Los resultados permitirán respaldar el proceso de evaluación crediticia y facilitar la toma de decisiones orientadas a una gestión más eficiente de la cartera de créditos.


## Dataset y Variables

* **Fuente:** Give Me Some Credit (Kaggle).
* **Variable objetivo (`SeriousDlqin2yrs`):** Indicador binario (`1` = retraso de 90 días o más; `0` = sin impago grave).
* **Variables predictoras (10 variables independientes):** Utilización de líneas revolventes (`RevolvingUtilizationOfUnsecuredLines`), edad (`age`), historial de atrasos (`30-59`, `60-89` y `90+` días), *DebtRatio*, ingreso mensual (`MonthlyIncome`), líneas de crédito/préstamos abiertos, préstamos hipotecarios/inmobiliarios y número de dependientes.


## Metodología y Flujo del Proyecto

1. **Análisis Exploratorio de Datos (EDA):** Diagnóstico del desbalance de clases (alta proporción de buenos pagadores frente a casos de default) y presencia de valores nulos en variables clave.
2. **Preparación y Partición Estratificada:** División *train/test* (`80/20`) con `stratify=y` para preservar la tasa real de impago en ambos conjuntos.
3. **Modelado y Manejo de Desbalance:** Entrenamiento y comparación de **Random Forest**, **XGBoost** y **LightGBM** utilizando ponderación de clases (`class_weight` / `scale_pos_weight`).
4. **Evaluación Financiera:** Medición mediante **ROC-AUC**, **Estadística KS (Kolmogorov-Smirnov)** y reporte de clasificación (`precision`, `recall`, `f1-score`).
5. **Optimización de Umbral Financiero:** Simulación de costos y beneficios por escenario para identificar el punto de corte que maximiza la utilidad neta estimada frente al umbral por defecto de `0.50`.
6. **Explicabilidad con SHAP:** Análisis global (*Summary Plot*) y local (*Waterfall Plot*) para saber qué variables impulsan el riesgo a nivel de cartera y para solicitantes individuales.


## Resultados Principales

| Modelo | ROC-AUC | Estadística KS | Recall (Default) | Observaciones |
| :--- | :---: | :---: | :---: | :--- |
| **Random Forest** | 0.8411 | 53.02% | 0.16 | Menor sensibilidad a *defaults* con umbral 0.50 |
| **XGBoost** | 0.8697 | 58.73% | 0.79 | Alta discriminación y detección de morosidad |
| **LightGBM** | 0.8690 | 58.51% | 0.77 | Desempeño equiparable a XGBoost con alta eficiencia |

* **Selección:** **XGBoost** y **LightGBM** lideran en discriminación (*ROC-AUC ~0.87* y *KS > 58%*) y en cobertura de casos de *default*.
* **Ajuste de umbral:** Calibrar el *threshold* mediante una matriz de costo-beneficio permite alinear la regla de decisión con el impacto financiero real en lugar de depender de un corte estadístico arbitrario.


## Explicabilidad (SHAP)

* **Nivel global:** El uso intensivo de líneas revolventes (`RevolvingUtilizationOfUnsecuredLines`) y los antecedentes de atrasos (`90DaysLate`, `60-89DaysPastDue`, `30-59DaysPastDue`) destacan entre los factores que más elevan el riesgo de impago.
* **Nivel individual:** Los gráficos *Waterfall* permiten contrastar perfiles de bajo riesgo (baja utilización y sin morosidad previa) frente a perfiles de alto riesgo (múltiples atrasos acumulados y alta utilización de crédito), facilitando la trazabilidad de cada decisión.
