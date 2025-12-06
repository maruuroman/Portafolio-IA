# Titanic con Ensambles: Random Forest vs Gradient Boosting. 
## Comparo ensambles de árboles (RF vs GB) sobre Titanic con pipeline, búsqueda de hiperparámetros, métricas, importancia de features y partial dependence plots (PDP).

## objetivo 
Comparar Random Forest y Gradient Boosting sobre Titanic con `Pipeline` + `ColumnTransformer`, GridSearchCV, métricas de clasificación, importancia de variables por permutación y Partial Dependence Plots (PDP).

## Datos y preparación
- Fuente: `fetch_openml("titanic", version=1)`.
- Limpieza mínima (dropna) sobre `pclass, sex, age, sibsp, parch, fare, embarked`.
- `train_test_split` estratificado (80/20).

## Modelado
- Preprocesamiento con `StandardScaler` para numéricas y `OneHotEncoder` para categóricas.
- Búsqueda de hiperparámetros:
  - RF: `n_estimators ∈ {150,300}`, `max_depth ∈ {None,5,8}`.
  - GB: `n_estimators ∈ {100,200}`, `learning_rate ∈ {0.05,0.1}`, `max_depth ∈ {2,3}`.
- Métrica de CV: `f1`.

## Evaluación
- Classification report (por clase y ponderado).
- ROC-AUC y curva ROC.
- Permutación de importancias y PDP para `age` y `fare`.

## Hallazgos (resumen)
- Ambos ensambles rinden bien como *baselines* robustos; GB puede superar a RF en datasets con señales suaves y buen *tuning* de `learning_rate`.
- Edad y tarifa suelen emerger como variables influyentes; las PDP permiten interpretar el efecto marginal.

## Próximos pasos
- Balanceo de clases (class_weight, SMOTE).
- *Feature engineering* específico (familiares, cabinas, títulos).
- Calibración de probabilidades y análisis de *thresholds*.

## Evidencia 

- En el archivo [Trabajo1](Trabajo1.ipynb) se encuantran realizada la actividad.