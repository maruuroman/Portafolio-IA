---
title: "De Perceptrón a Redes Neuronales – Práctica 7"
date: 2025-01-01
---
## Contexto
Esta práctica forma parte de la Unidad 2 del curso, enfocada en la transición desde los modelos lineales clásicos hacia las redes neuronales artificiales.
El objetivo general fue comprender cómo el perceptrón, uno de los primeros modelos de aprendizaje automático, sirve como base conceptual para las redes neuronales multicapa (MLP), que permiten resolver problemas no lineales.

A través de ejercicios prácticos, se implementó un perceptrón desde cero y luego se exploró la arquitectura básica de un MLP utilizando TensorFlow y Keras, observando cómo las capas ocultas y las funciones de activación influyen en el aprendizaje.

## Objetivos
- Comprender la estructura y funcionamiento del perceptrón simple.
- Implementar el cálculo de salida a partir de entradas, pesos y bias.
- Visualizar gráficamente la frontera de decisión del modelo.
- Extender el concepto a un perceptrón multicapa (MLP).
- Analizar cómo las funciones de activación y la profundidad de la red afectan la capacidad de clasificación.

## Actividades Realizadas

1. Implementación del Perceptrón Simple
Se comenzó con la función perceptron(x1, x2, w1, w2, bias), encargada de calcular una salida binaria (0 o 1) según el valor de activación lineal.
Esta implementación permitió experimentar con distintos valores de pesos y bias para observar cómo la frontera de decisión cambia.

2. Visualización de Resultados
Se utilizó la función graficar_perceptron() para mostrar gráficamente los puntos de entrada y la línea divisoria aprendida.
Los puntos correctamente clasificados se marcaron en *azul, y los incorrectos en **rojo*, facilitando la interpretación visual del desempeño del modelo.

3. Transición al Perceptrón Multicapa (MLP)
Posteriormente se introdujo la idea de una red neuronal con múltiples capas densas.
A partir de la base teórica del perceptrón, se construyó una red Sequential con *capas densas (Dense)* y *funciones de activación ReLU y softmax*, utilizando TensorFlow/Keras.
El modelo se entrenó con un conjunto de datos ya preprocesado, dividiendo entre *train, **validation* y *test*, siguiendo la misma metodología que en prácticas anteriores.

4. Entrenamiento y Evaluación
El modelo se compiló con optimizer='adam' y loss='sparse_categorical_crossentropy', utilizando accuracy como métrica principal.
Durante el entrenamiento se observó la evolución del error y la precisión, comprobando cómo el MLP logra representar patrones más complejos que el perceptrón simple.

## Desarrollo y Resultados

- El perceptrón simple permitió *visualizar la frontera de decisión lineal*, demostrando que solo puede separar clases linealmente separables.
- Al pasar al MLP, el modelo *incrementó su capacidad de clasificación*, logrando adaptarse a relaciones no lineales entre las variables de entrada.
- Se comprobó que las *funciones de activación no lineales* (como ReLU o tanh) son las que posibilitan este aprendizaje más complejo.
- Se realizaron visualizaciones que mostraron la mejora del modelo al aumentar el número de capas o neuronas, aunque también se notó un aumento del tiempo de entrenamiento.

## Evidencias
- En el archivo [Practica7](07-Practica7.ipynb) se encuantra:  
- Código del perceptrón simple implementado desde cero.
- Función para graficar y visualizar la frontera de decisión.
- Entrenamiento y evaluación de una red MLP en TensorFlow.
- Gráficos comparativos de desempeño entre modelos.

## Reflexión Personal
Esta práctica me ayudó a entender con claridad cómo funciona una red neuronal desde sus fundamentos.
Al implementar el perceptrón manualmente y luego pasar al MLP, pude ver cómo los conceptos teóricos se traducen en código y resultados concretos.

Aprendí que:
- Las redes profundas son una extensión natural del perceptrón simple.
- Las funciones de activación juegan un rol clave al permitir que el modelo aprenda relaciones no lineales.
- Un diseño de red más complejo no siempre garantiza mejor rendimiento, y requiere ajustar hiperparámetros cuidadosamente.

Fue una práctica muy útil para afianzar los conceptos previos y comenzar a comprender cómo se estructuran los modelos de aprendizaje profundo.


## Referencias

- Documentación oficial de TensorFlow/Keras:
  - [tf.keras.layers.Dense](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense)
  - [tf.keras.activations](https://www.tensorflow.org/api_docs/python/tf/keras/activations)
- Notas de clase: Unidad 2 – Redes Neuronales y Backpropagation.
- McCulloch, W. S., & Pitts, W. (1943). A Logical Calculus of Ideas Immanent in Nervous Activity.