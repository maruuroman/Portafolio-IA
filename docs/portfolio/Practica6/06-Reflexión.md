---
title: "Clustering y PCA - Mall Customer Segmentation – Práctica 6"
---

# Clustering y PCA - Mall Customer Segmentation – Práctica 6

## Contexto
En esta práctica trabajé con técnicas de aprendizaje no supervisado, aplicando distintos algoritmos de clustering y métodos de selección de características sobre un dataset multivariado.  
El objetivo fue comparar el desempeño de varios enfoques de agrupamiento y comprender cómo la reducción o selección de variables afecta la calidad de los clusters.  
La actividad se desarrolló en Google Colab con Python, utilizando principalmente *pandas*, *numpy*, *scikit-learn*, *matplotlib* y *seaborn*.

## Objetivos
- Aplicar y comparar diferentes algoritmos de clustering (K-Means, DBSCAN, HDBSCAN, Gaussian Mixture, Spectral y Agglomerative).  
- Evaluar la calidad de los clusters mediante métricas como Silhouette Score.  
- Experimentar con selección de características (forward y backward) para mejorar la separación de grupos.  
- Integrar preprocesamiento (escalado, PCA) en un pipeline reproducible.

## Actividades (con tiempos estimados)
| Actividad                                   | Tiempo | Resultado esperado                                   |
|----------------------------------------------|:------:|------------------------------------------------------|
| Revisión teórica de clustering y métricas    |  30m  | Identificación de algoritmos y criterios de evaluación|
| Preparación y limpieza del dataset           |  20m  | Datos escalados, sin valores faltantes               |
| Implementación de K-Means y búsqueda de *k*  |  30m  | Selección de k óptimo (método del codo, Silhouette)  |
| Pruebas con DBSCAN / HDBSCAN                 |  40m  | Ajuste de `eps` y `min_samples`                      |
| Gaussian Mixture y clustering jerárquico     |  30m  | Comparación de clusters y probabilidades             |
| Selección de características (forward/back)  |  30m  | Subconjunto de variables que maximizan la métrica    |
| Análisis y visualización de resultados       |  30m  | Gráficos 2D/3D, interpretación de patrones           |

## Desarrollo
- **Preprocesamiento:** Escalé las variables numéricas y exploré correlaciones para detectar redundancias.  
- **K-Means:** Probé distintos valores de *k*, usando el “método del codo” y Silhouette Score para identificar el número óptimo de clusters.  
- **DBSCAN / HDBSCAN:** Ajusté parámetros (`eps`, `min_samples`) para controlar el ruido y obtener agrupamientos significativos.  
- **Otros algoritmos:** Implementé Gaussian Mixture, Spectral y Agglomerative, comparando su desempeño y visualizando resultados en 2D con PCA.  
- **Selección de características:** Apliqué *forward* y *backward selection* para encontrar subconjuntos de variables que aumentaran la separación de grupos.  
- **Evaluación:** Registré Silhouette Score y gráficos de dispersión para cada combinación, destacando que la reducción de dimensiones mejoró la claridad de algunos clusters.

## Evidencias
- En el archivo [Practica6](06-Practica6.ipynb) se encuantran realizada la actividad.  
- Gráficos generados:
  - Curva del método del codo y Silhouette para K-Means.  
  - Visualización 2D PCA de los distintos algoritmos.  
  - Mapas de calor de correlaciones antes y después de la selección de características.

## Reflexión
- **Aprendizajes:**  
  - Comprendí las diferencias entre algoritmos de clustering.  
  - Vi cómo la selección de características puede mejorar la calidad de los grupos y simplificar la interpretación.   

## Referencias
- Documentación de [scikit-learn](https://scikit-learn.org/stable/modules/clustering.html)  
- Artículos de *Silhouette Score* y selección de características en [scikit-learn](https://scikit-learn.org/stable/modules/feature_selection.html)  
- Apuntes de clase.
- Se encuantran en el archivo "06-Práctica6.ipynb" dentro de esta carpeta.