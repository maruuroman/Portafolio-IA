---
title: "Práctica 5 – Validación y Selección de Modelos"
date: 2025-01-01
---

# Práctica 5 – Validación y Selección de Modelos  
**Materia:** Introducción a los Métodos de Aprendizaje Automático  

## Contexto
Esta práctica forma parte de la Unidad 1 de *Machine Learning Clásico* y se centra en el ciclo completo de validación y selección de modelos para problemas de clasificación multiclase.  
El trabajo se realizó sobre el dataset Student Dropout and Academic Success del *UCI Machine Learning Repository*, cuyo objetivo es predecir el éxito académico o abandono estudiantil en educación superior, a partir de 36 características demográficas, académicas y socioeconómicas.

## Objetivos
- Prevenir data leakage utilizando *pipelines*.
- Implementar validación cruzada robusta (KFold y StratifiedKFold).  
- Comparar múltiples algoritmos de forma sistemática.  
- Analizar métricas de estabilidad y seleccionar el modelo más adecuado.  
- Comprender la importancia de la explicabilidad en modelos predictivos.

## Actividades Realizadas
1. **Preparación de datos**  
   - Carga del dataset desde `ucimlrepo`.
   - Análisis de distribución de clases.
   - Mapeo de etiquetas categóricas a valores numéricos.

2. **Validación Cruzada**  
   - Creación de un *pipeline* con `StandardScaler` y `LogisticRegression`.
   - Comparación de `KFold` y `StratifiedKFold` mediante `cross_val_score` para medir accuracy y estabilidad.
   - Visualización de la distribución de scores con boxplots.

3. **Competencia de Modelos**  
   - Evaluación de tres candidatos:  
     - Logistic Regression.  
     - Ridge Classifier.  
     - Random Forest.  
   - Uso de validación cruzada estratificada de 5 folds.
   - Cálculo de media y desviación estándar de accuracy para elegir el ganador.

4. **Optimización de Hiperparámetros**  
   - Aplicación de `GridSearchCV` y `RandomizedSearchCV` al mejor modelo.  
   - Comparación de eficiencia y rendimiento final con cross-validation.

5. **Explicabilidad**  
   - Análisis de feature importance con `RandomForestClassifier`.  
   - Agrupación de variables en categorías académicas, demográficas y económicas.  
   - Interpretación de las características más influyentes en la predicción de abandono.  
   - Ejemplo de predicción individual y visualización de árboles de decisión.

6. **Reflexión Teórica**  
   - Discusión de conceptos clave:  
     - Data leakage: filtración de información del conjunto de prueba en el entrenamiento.  
     - Diferencias entre `KFold` y `StratifiedKFold`.  
     - Interpretación de resultados tipo “95.2 % ± 2.1 %”.  
     - Razones por las que Random Forest no requiere escalado.  
     - Relevancia de la estabilidad vs. máximo accuracy.

## Desarrollo y Resultados
- **Distribución de clases:** desbalance moderado (Dropout < Enrolled/Graduate).  
- **Validación cruzada:**  
  - `StratifiedKFold` presentó menor desviación estándar que `KFold`, por lo que se recomendó su uso.  
- **Competencia de modelos:**  
  - El **Random Forest** obtuvo el mejor *accuracy* promedio y gran estabilidad.  
- **Optimización:**  
  - Ajuste de hiperparámetros (n_estimators, max_depth, min_samples_split) mejoró ligeramente la media de *accuracy*.
- **Explicabilidad:**  
  - Las variables académicas (unidades aprobadas, promedios de calificaciones) fueron las más determinantes.  
  - Factores económicos y demográficos tuvieron menor peso.

## Evidencias
- Notebook “Práctica 5 – Validación y Selección de Modelos” con:
  - Código de *pipelines*, validación cruzada y comparación de modelos.  
  - Gráficos de distribución de *scores* y *feature importance*.  
  - Visualizaciones de árboles individuales del Random Forest.

## Reflexión Personal
Esta práctica consolidó mi comprensión del proceso de selección de modelos y de la importancia de la **estabilidad** sobre la mera búsqueda de un *accuracy* máximo.  
Aprendí a:
- Prevenir fugas de datos mediante *pipelines* integrales.
- Seleccionar validaciones que respeten el balance de clases.
- Combinar rendimiento y explicabilidad para generar modelos confiables en contextos educativos reales.

## Referencias
- UCI Machine Learning Repository – [Student Dropout and Academic Success Dataset](https://archive.ics.uci.edu/).  
- Documentación de scikit-learn:  
  - [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)  
  - [`cross_val_score`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)  
  - [`GridSearchCV`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)  
  - [`RandomForestClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)  
- Se encuantran en el archivo "05-Práctica5.ipynb" dentro de esta carpeta.
