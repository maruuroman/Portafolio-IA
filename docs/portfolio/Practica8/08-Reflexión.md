---
title: "Backpropagation y Optimizadores – Práctica 8 "
date: 2025-01-01
---
# Backpropagation y Optimizadores – Práctica 8
## Contexto
Esta práctica corresponde a la Unidad 2 del curso y tuvo como objetivo principal introducir los fundamentos del entrenamiento de redes neuronales mediante backpropagation, así como el rol de los optimizadores en el proceso de ajuste de los pesos.
El ejercicio se desarrolló utilizando TensorFlow y Keras, implementando una red neuronal multicapa (MLP) entrenada sobre el dataset CIFAR-10, que contiene 60.000 imágenes a color de 32×32 píxeles distribuidas en 10 clases.
El propósito fue construir un modelo de clasificación multiclase capaz de identificar el tipo de objeto presente en cada imagen.

## Objetivos
- Comprender el flujo de entrenamiento de una red neuronal: forward pass y backpropagation.
- Implementar un modelo MLP con TensorFlow/Keras.
- Aplicar un optimizador Adam y entender su funcionamiento.
- Evaluar la precisión del modelo sobre los conjuntos de entrenamiento y prueba.
- Visualizar el proceso de entrenamiento mediante TensorBoard.

## Actividades Realizadas
1. Preparación del entorno y librerías
Se importaron las librerías necesarias para la manipulación de datos, visualización y construcción de redes neuronales:

import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
import numpy as np, matplotlib.pyplot as plt, os, datetime as dt

Además, se fijó una semilla aleatoria para asegurar la reproducibilidad de los resultados.

2. Carga y preprocesamiento de datos
Se utilizó el dataset CIFAR-10, disponible directamente desde keras.datasets.
Las etiquetas fueron convertidas a vectores simples y los valores de los píxeles se normalizaron al rango [-1, 1], lo cual mejora la estabilidad del entrenamiento.

(x_train, y_train), (x_test, y_test) = keras.datasets.cifar10.load_data()
y_train = y_train.flatten()
y_test = y_test.flatten()

x_train = (x_train.astype("float32") / 255.0 - 0.5) * 2.0
x_test = (x_test.astype("float32") / 255.0 - 0.5) * 2.0

Luego, se separó un 10 % de los datos de entrenamiento para validación y se aplanaron las imágenes de 32×32×3 a vectores de tamaño 3072 para poder emplearlas en capas densas.

3. Definición del modelo
Se construyó una red neuronal simple utilizando la API Sequential de Keras, con dos capas ocultas densas de 32 neuronas cada una y activación ReLU, seguidas de una capa de salida con activación softmax:

model = keras.Sequential([
    layers.Dense(32, activation='relu', input_shape=(x_train.shape[1],)),
    layers.Dense(32, activation='relu'),
    layers.Dense(10, activation='softmax')
])

4. Compilación y entrenamiento
El modelo se compiló utilizando el optimizador Adam, la función de pérdida sparse_categorical_crossentropy (adecuada para etiquetas enteras) y la métrica de accuracy:

model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

El entrenamiento se realizó durante 5 épocas con un tamaño de lote (batch_size) de 32.
Se habilitó TensorBoard para registrar la evolución de las métricas y visualizar histogramas de pesos.

run_dir = os.path.join("tb_logs", "experiment" + dt.datetime.now().strftime("%Y%m%d-%H%M%S"))
history = model.fit(
    x_train, y_train,
    epochs=5,
    batch_size=32,
    validation_data=(x_test, y_test),
    callbacks=[keras.callbacks.TensorBoard(log_dir=run_dir, histogram_freq=1)]
)

5. Evaluación del modelo
Al finalizar el entrenamiento, el modelo fue evaluado en los conjuntos de entrenamiento y prueba:

train_loss, train_acc = model.evaluate(x_train, y_train, verbose=0)
test_loss, test_acc = model.evaluate(x_test, y_test, verbose=0)

## Resultados obtenidos (valores aproximados):
|Conjunto	     | Pérdida	| Precisión |
|Entrenamiento |	 1.23 	|   0.52    |
|Prueba	       |   1.45	  |   0.45    |

El rendimiento fue moderado, lo cual se considera razonable dado que la arquitectura utilizada es simple para un dataset visual complejo como CIFAR-10.

## Desarrollo y Resultados
El modelo logró aprender patrones básicos de las imágenes, aunque su precisión en el conjunto de prueba se mantuvo limitada debido a la falta de profundidad y regularización.
Se verificó, sin embargo, que el proceso de backpropagation funcionaba correctamente y que el modelo era capaz de ajustar los pesos para reducir la función de pérdida a lo largo de las épocas.
A través de TensorBoard, fue posible observar las curvas de entrenamiento y validación, confirmando la correcta ejecución del flujo de optimización.

## Reflexión 
Backpropagation es el mecanismo que permite actualizar los pesos de la red en función del error cometido. Se basa en el cálculo del gradiente de la pérdida respecto a cada parámetro.
El optimizador Adam combina las ventajas del momentum y la adaptación del learning rate, acelerando la convergencia.
La elección de funciones de activación no lineales (como ReLU) permite que la red aprenda representaciones más complejas.
En este caso, la simplicidad de la red MLP limita la capacidad del modelo frente a un conjunto de imágenes con alta variabilidad, como CIFAR-10, donde suelen emplearse redes convolucionales.

## Evidencias
- En el archivo [Practica8](08-Practica8.ipynb) contiene:
- Código de carga, preprocesamiento y normalización de datos.
- Definición, compilación y entrenamiento del modelo MLP.
- Configuración de TensorBoard y visualización de métricas.
- Evaluación final del modelo sobre los conjuntos de entrenamiento y prueba.

## Reflexión Personal
Esta práctica me permitió comprender cómo se entrena una red neuronal desde cero y el papel clave del backpropagation en el ajuste de los pesos.
Pude observar cómo el uso del optimizador Adam facilita el aprendizaje sin necesidad de un ajuste manual complejo.
Además, aprendí a interpretar las curvas de entrenamiento y validación en TensorBoard, lo que ayuda a identificar posibles casos de sobreajuste o estancamiento del aprendizaje.

## Referencias
- TensorFlow API Documentation:
  - Dense Layer
  - Optimizers
  - Callbacks (TensorBoard)
- CIFAR-10 Dataset – Keras Datasets
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press
