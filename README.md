# Metodología con IA para mantenimiento predictivo en turbomaquinaria

**Universidad Internacional de La Rioja (UNIR)**

**Máster de Formación Permanente en Inteligencia Artificial en Procesos Industriales y de Manufactura**

**Autores:** Sergio Martinez, Cristhian Diaz, Manuel A. Rodriguez

---

##  Resumen / Abstract
Este proyecto evalúa la viabilidad de modelos de Inteligencia Artificial (IA) para la detección temprana de fallos y la estimación de la Vida Útil Restante (RUL) en turbinas de aviacion e industriales. 

La metodología desarrolla un enfoque dual que contrasta dos aproximaciones analíticas:
*   Modelos de regresión lineal y no lineal (Random Forest) para evaluar trayectorias de degradación continua utilizando el dataset aeronáutico C-MAPSS.
*   Ensamblajes de clasificación (Gradient Boosting) orientados a identificar anomalías discretas en turbinas de gas estacionarias.

Para robustecer los modelos, se aplican técnicas de aumento de datos mediante inyección de ruido gaussiano, balanceo sintético con SMOTE e inteligencia artificial explicable (SHAP) para traducir las predicciones estocásticas en diagnósticos auditables en planta.

---

##  Objetivos del Proyecto
*   Diseñar y evaluar una metodología de inteligencia artificial para el mantenimiento predictivo de motores aeronáuticos y turbomaquinaria.
*   Combinar la detección temprana de fallos mediante clasificación con la estimación de vida útil restante mediante regresión.
*   Analizar el impacto del preprocesado, la extensión de datos (SMOTE) y la selección algorítmica sobre el rendimiento predictivo de los modelos.
*   Incorporar una capa de explicabilidad algorítmica basada en SHAP para interpretar la contribución de las variables operacionales a las predicciones del sistema.

---

##  Arquitectura Metodológica (Pipeline)

El desarrollo analítico se estructura en cinco etapas iterativas basadas en el ciclo de vida CRISP-DM y MLOps:

**Etapa 1: Ingesta de datos y formulación matemática**
*   Se calcula la Vida Útil Restante de forma diferenciada según la naturaleza de la fuente de datos. 
*   Para los datasets tipo C-MAPSS, el RUL se formula de la siguiente manera:
$$RUL_t=max(t_{motor})-t$$

**Etapa 2: Limpieza de datos e imputación**
*   Se aplica una heurística de limpieza robusta que incluye el filtrado de registros duplicados e incompletos.
*   Se descartan variables de varianza cero y se normalizan los datos para garantizar el correcto cálculo de pesos algorítmicos.

**Etapa 3: Ingeniería de Características (Feature Engineering)**
*   Se extraen operadores temporales diferenciales para medir la tasa de variación instantánea:
$$\Delta x_t=x_t-x_{t-1}$$
*   Se introduce aumento de datos mediante ruido blanco gaussiano para fortalecer la inmunidad del modelo:
$$x=x+\epsilon$$
*   La variable de ruido se distribuye estadísticamente de la siguiente forma:
$$\epsilon\sim\mathcal{N}(0,0.01\cdot\sigma_x)$$

**Etapa 4: Construcción y Entrenamiento del Modelo Predictivo**
*   Se establecen líneas base lineales y modelos ensamblados no lineales como Random Forest Regressor para entornos de degradación continua.
*   Se emplean clasificadores tipo Gradient Boosting para el dataset de turbinas a gas.
*   Se reduce el umbral probabilístico operativo al 25% para priorizar la detección temprana de fallos frente a la exactitud global.

**Etapa 5: Evaluación y Visualización**
*   Se extraen métricas continuas de error como RMSE, MAE y coeficiente de determinación.
*   Se extraen métricas operacionales de clasificación mediante Precision, Recall, F1 score y matriz de confusión.

---

##  Resultados Principales

| Dominio Operativo | Enfoque de Modelado | Conclusiones y Hallazgos |
| :--- | :--- | :--- |
| **Simulación Turbofán (C-MAPSS)** | Regresión Continua (Random Forest) | Random Forest superó consistentemente a la regresión lineal, manejando eficientemente el desgaste terminal no lineal y alcanzando su máximo nivel de varianza explicada en el subconjunto FD003. |
| **Turbinas de Gas Estacionarias** | Clasificación de Anomalías (Gradient Boosting) | La regresión convencional fracasó al carecer de una degradación continua monótona. Al calibrar el umbral de decisión clasificador al 25%, el modelo priorizó la mitigación del riesgo y maximizó la captura de fallos incipientes (Recall). |
| **Generalización Unificada** | Modelo Híbrido | Se demostró empíricamente la inviabilidad de un modelo predictivo universal debido a la divergencia estructural del espacio de características y a conflictos en la optimización de la función de pérdida. |

---

##  Tecnologías y Librerías Empleadas
*   **Lenguaje:** Python 3.10+.
*   **Manipulación de Datos y Álgebra:** `pandas`, `numpy`.
*   **Machine Learning y Preprocesado:** `scikit-learn`, `xgboost`.
*   **Aumento de Datos:** `imbalanced-learn` (SMOTE).
*   **Explicabilidad (XAI):** `shap`.
*   **Visualización Científica:** `matplotlib`, `seaborn`. 