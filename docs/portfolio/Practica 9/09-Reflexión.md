# CNNs y Transfer Learning con TensorFlow/Keras – Práctica 9

## Contexto
Esta práctica tuvo como finalidad aplicar los conceptos de **redes neuronales convolucionales (CNNs)** y **Transfer Learning** utilizando TensorFlow y Keras.  
Se trabajó con el dataset **CIFAR-10**, compuesto por imágenes distribuidas en 10 clases de objetos.  
El objetivo fue **comparar el desempeño entre una CNN desarrollada desde cero** y un modelo **preentrenado con MobileNetV2**, evaluando su precisión, pérdida y capacidad de generalización.


## Objetivos
- Implementar una **CNN simple** para clasificación de imágenes.  
- Aplicar **Transfer Learning** con un modelo preentrenado en ImageNet.  
- Entrenar y evaluar ambos modelos sobre CIFAR-10.  
- Analizar los resultados obtenidos y el efecto del fine-tuning.  
- Comprender las ventajas del uso de arquitecturas preentrenadas.

---

## Actividades Realizadas
1. Configuración del entorno TensorFlow/Keras y detección de GPU.  
2. Carga del dataset CIFAR-10 con `keras.datasets.cifar10.load_data()`.  
3. Normalización de imágenes y conversión de etiquetas a formato one-hot.  
4. Implementación de una CNN simple con dos capas convolucionales (`Conv2D`) y pooling (`MaxPooling2D`).  
5. Creación de un modelo de **Transfer Learning** basado en **MobileNetV2** con pesos de ImageNet.  
6. Compilación y entrenamiento de ambos modelos con **EarlyStopping**.  
7. Evaluación final, comparación de métricas y análisis de overfitting.

---

## Desarrollo y Resultados
### CNN Simple
La red creada desde cero se estructuró con:
```python
layers.Conv2D(32, (3,3), padding='same')
layers.MaxPooling2D((2,2))
layers.Conv2D(64, (3,3), padding='same')
layers.MaxPooling2D((2,2))
optimizer=optimizers.Adam(learning_rate=0.001)

## Transfer Learning
Se utilizó MobileNetV2 como base no entrenable:
base_model = applications.MobileNetV2(
    weights='imagenet',
    include_top=False,
    input_shape=(32,32,3)
)
optimizer=optimizers.Adam(learning_rate=0.001)

Ambos modelos fueron entrenados durante 10 épocas con validación y callback de EarlyStopping.
Los resultados mostraron una clara ventaja del modelo con Transfer Learning:

Modelo	Precisión Test	Pérdida Test
CNN Simple	~0.73	~0.85
Transfer Learning (MobileNetV2)	~0.86	~0.55

El modelo basado en MobileNetV2 logró mayor precisión y menor pérdida, evidenciando mejor generalización y menor sobreajuste.

## Reflexión
Esta práctica permitió comprobar que entrenar una CNN desde cero ofrece una comprensión más profunda de su arquitectura, pero los modelos preentrenados como MobileNetV2 proporcionan un rendimiento muy superior con menos tiempo de entrenamiento.
El Transfer Learning es especialmente útil cuando se dispone de datasets pequeños o con variabilidad limitada, aprovechando características aprendidas por modelos entrenados en grandes volúmenes de datos.

## Evidencias
- Implementación de create_simple_cnn() y create_transfer_model() en TensorFlow/Keras.
- Compilación y entrenamiento con optimizers.Adam y EarlyStopping.
- Resultados numéricos y visuales de precisión y pérdida.
- Notebook 09-Practica9.ipynb con todo el código ejecutado.

## Reflexión Personal
Esta práctica me ayudó a consolidar los conceptos de arquitectura CNN y Transfer Learning, entendiendo mejor cómo las redes preentrenadas aceleran el aprendizaje y mejoran la precisión.
Pude observar las ventajas del fine-tuning y la importancia de ajustar correctamente el learning rate.
En general, fue una experiencia muy enriquecedora que reforzó mis conocimientos sobre visión por computadora y deep learning aplicado.

## Referencias 
- TensorFlow / Keras Documentation (2024)
    - CIFAR-10 Dataset
    - MobileNetV2
    - Optimizers – Adam
    - Callbacks – EarlyStopping
- Chollet, F. (2018). Deep Learning with Python. Manning Publications.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press.