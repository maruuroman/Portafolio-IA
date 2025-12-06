# Regresion Lineal y Regresion Logistica - Práctica 4 

## Contexto
En esta práctica se trabajó con técnicas básicas de aprendizaje supervisado para comprender el flujo completo:  
preprocesamiento de datos, división en conjuntos de entrenamiento/prueba, entrenamiento de modelos y evaluación de métricas.  
El objetivo fue reforzar los conceptos vistos en clase y aplicar modelos de clasificación con Python y librerías comunes.

## Objetivos
- Implementar y evaluar al menos dos algoritmos de clasificación sobre un dataset provisto.  
- Medir exactitud, matriz de confusión y curva ROC para comparar el desempeño de los modelos.  
- Documentar el proceso en un notebook reproducible.

## Actividades (con tiempos estimados)
| Actividad                               | Tiempo |
|------------------------------------------|:------:|
| Exploración del dataset                  | 30 min |
| Limpieza y preprocesamiento (normalizado) | 45 min |
| Entrenamiento de modelo base (*k*-NN)    | 40 min |
| Entrenamiento de regresión logística     | 40 min |
| Evaluación y comparación de métricas     | 30 min |
| Redacción y comentarios en el notebook   | 35 min |

## Desarrollo
Se utilizó Jupyter Notebook con `pandas`, `scikit-learn` y `matplotlib`.  
1. **Exploración**: análisis de variables, valores faltantes y distribución de clases.  
2. **Preprocesamiento**: normalización de atributos numéricos y división 80/20 en train/test.  
3. **Modelado**:  
   - *k*-NN con búsqueda de hiperparámetro *k* mediante validación cruzada.  
   - Regresión logística con regularización L2.  
4. **Evaluación**: exactitud, matriz de confusión, precisión/recall y curva ROC.  
   La regresión logística obtuvo una exactitud ligeramente superior y mejor área bajo la curva.

## Evidencias
- - En el archivo [Practica4](04-Practica4.ipynb) se encuantran realizada la actividad.

## Reflexión
Aprendí a comparar modelos de forma sistemática y a valorar la importancia del preprocesamiento.  
En futuras prácticas dedicaré más tiempo a la exploración inicial para detectar posibles outliers.

## Referencias
- Documentación oficial de [scikit-learn](https://scikit-learn.org/stable/).  
- Notas de clase sobre evaluación de modelos.

