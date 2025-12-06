---
title: "Feature Engineering y Modelo Base – Práctica 2"
date: 2025-01-08
---

# Feature Engineering y Modelo Base – Práctica 2

## Contexto
Segunda práctica del proyecto Titanic en Google Colab, centrada en feature engineering y en la creación de un modelo de clasificación base.  
El objetivo es preparar los datos, generar nuevas variables que mejoren el aprendizaje y comparar un baseline (DummyClassifier) con un modelo LogisticRegression.

## Objetivos
- Aplicar feature engineering para enriquecer el dataset (nuevas columnas, imputación de valores).  
- Entrenar un modelo de Regresión Logística y compararlo con un DummyClassifier como baseline.  
- Evaluar el desempeño usando métricas de clasificación y matriz de confusión.

## Actividades (con tiempos estimados)
| Actividad                                         | Tiempo | Resultado esperado                                       |
|---------------------------------------------------|:------:|----------------------------------------------------------|
| Investigación de componentes de scikit-learn      | 10 min | Comprensión de LogisticRegression, DummyClassifier, métricas |
| Preprocesamiento e imputación de valores faltantes | 15 min | Dataset limpio sin NA                                     |
| Creación de nuevas features                        | 15 min | Columnas `FamilySize`, `IsAlone`, `Title`                 |
| Codificación y preparación de datos (get_dummies)  | 10 min | Matriz X lista para el modelo                             |
| Entrenamiento y evaluación de modelos              | 20 min | Métricas de baseline vs. Logistic Regression              |

## Desarrollo
- **Investigación inicial:**  
  Revisé la documentación de `LogisticRegression`, `DummyClassifier`, `train_test_split` y métricas de `classification_report` en la guía de usuario de scikit-learn.  
  - *LogisticRegression* se usa para problemas de clasificación binaria/multiclase.  
  - *DummyClassifier* sirve como modelo de referencia con estrategias simples (`most_frequent`, `stratified`…).  
  - El parámetro `stratify` en `train_test_split` mantiene la proporción de clases.  
  - Métricas clave: *accuracy*, *precision*, *recall*, *f1-score*.

- **Preprocesamiento:**  
  - Imputé valores faltantes de `Embarked` (moda), `Fare` (mediana) y `Age` (mediana agrupada por `Sex` y `Pclass`).

- **Feature Engineering:**  
  - `FamilySize` = `SibSp` + `Parch` + 1.  
  - `IsAlone` (1 si `FamilySize` = 1).  
  - `Title` extraído del nombre, agrupando títulos poco frecuentes en “Rare”.

- **Codificación:**  
  - Seleccioné variables clave y apliqué `pd.get_dummies` para variables categóricas, obteniendo una matriz de entrenamiento lista para el modelo.

- **Modelos y evaluación:**  
  - **Baseline:** `DummyClassifier(strategy="most_frequent")`.  
  - **Modelo base:** `LogisticRegression(max_iter=1000, solver="liblinear")`.  
  - División del dataset: 80 % entrenamiento, 20 % test, con `stratify=y` y `random_state=42`.  
  - Evalué *accuracy*, *classification_report* y *confusion_matrix*.

## Evidencias
- Se encuantran en el archivo "02-Práctica2.ipynb" dentro de esta carpeta. 
- Principales resultados (ejemplo típico en este dataset):
  - **Accuracy baseline:** ~0.62 (predice siempre “no sobrevivió”).  
  - **Accuracy Logistic Regression:** ~0.79.  
  - Matriz de confusión muestra más errores al predecir “sobrevive” cuando en realidad no lo hace (falsos positivos).


## Reflexión
- **Aprendizajes:**  
  - Un baseline es indispensable para validar mejoras; la Regresión Logística superó claramente al DummyClassifier.  
  - Features creadas como `FamilySize` y `Title` aportan información relevante para el modelo.  
  - Imputar valores según grupos (edad por sexo y clase) ayuda a preservar patrones.

- **Observaciones:**  
  - El modelo acierta más en la clase “no sobrevivió” que en “sobrevivió”.  
  - Los errores más frecuentes son falsos positivos (predice que sobrevivió pero no lo hizo).

- **Próximos pasos / mejoras:**  
  - Probar normalización de `Fare` y `Age`.  
  - Agregar interacciones entre variables (por ejemplo `Sex * Pclass`).  
  - Evaluar otros modelos (Random Forest, Gradient Boosting) para comparar desempeño.

## Referencias
- [Scikit-learn User Guide – Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)  
- [Scikit-learn User Guide – DummyClassifier](https://scikit-learn.org/stable/modules/model_evaluation.html#dummy-estimators)  
- [Titanic Kaggle Competition](https://www.kaggle.com/competitions/titanic)
