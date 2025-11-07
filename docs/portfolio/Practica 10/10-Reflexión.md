# Práctico 10: Data Augmentation Avanzado & Explicabilidad

## Contexto

Este práctico forma parte de la Unidad 3 del módulo de *Computer Vision*, y tiene como objetivo entrenar un modelo robusto capaz de clasificar correctamente especies de flores del dataset **Oxford Flowers102**, enfrentando condiciones variables de iluminación, ángulo y fondo.  
Además, busca aplicar **técnicas de explicabilidad** para comprender cómo el modelo toma sus decisiones, aumentando la confianza en sus predicciones.

El caso de negocio simula una **aplicación móvil para identificación de flores** destinada a jardineros y botánicos aficionados. El desafío consiste en lograr un modelo preciso, robusto y explicable que funcione correctamente con imágenes reales enviadas por usuarios.

---

## Objetivos

- Implementar un pipeline de **data augmentation avanzado** con TensorFlow/Keras.  
- Entrenar modelos con **transfer learning** usando arquitecturas preentrenadas (EfficientNetB0, MobileNetV2).  
- Aplicar técnicas de robustez y explicabilidad: **GradCAM** e **Integrated Gradients**.  
- Evaluar el impacto de augmentation en la precisión y estabilidad del modelo.  
- Comparar desempeño entre un modelo baseline y uno augmentado.

---

## Actividades realizadas

1. **Carga y preparación del dataset Oxford Flowers102** mediante `tensorflow_datasets`, aplicando `resize` y conversión a `float32`.  
2. **Definición del pipeline baseline** con normalización mediante `preprocess_input`.  
3. **Implementación de data augmentation avanzado** usando capas de Keras:
   ```python
   layers.RandomFlip("horizontal"),
   layers.RandomRotation(0.125),
   layers.RandomZoom(0.2),
   layers.RandomTranslation(0.1),
   layers.RandomBrightness(0.2),
   layers.RandomContrast(0.2)

4. Creación del modelo base con transfer learning:
base_model = keras.applications.EfficientNetB0(
    include_top=False,
    weights='imagenet',
    input_shape=(IMG_SIZE, IMG_SIZE, 3)
)
base_model.trainable = False

5. Entrenamiento del modelo con los datos aumentados:
history = model.fit(
    train_augmented,
    validation_data=test_baseline,
    epochs=5,
    verbose=1
)

6. Evaluación del desempeño y guardado del modelo (mi_modelo_flores.h5). 

## Desarrollo y Resultados

El modelo se entrenó durante 5 épocas usando transfer learning, logrando una alta accuracy en validación en pocas iteraciones.

El data augmentation mejoró la capacidad del modelo para generalizar, reduciendo el sobreajuste visible en el baseline.

Se comprobó que EfficientNetB0 ofrece un equilibrio adecuado entre precisión y velocidad de entrenamiento.

El modelo resultante mostró robustez ante rotaciones, cambios de brillo y fondos diversos.

## Reflexión

Este práctico permitió comprender el valor del data augmentation avanzado para mejorar la robustez de los modelos visuales frente a variaciones del entorno real.
Además, consolidó el uso de transfer learning, optimizando tiempo de entrenamiento y aprovechando conocimientos previos de arquitecturas preentrenadas.

El enfoque de explicabilidad (GradCAM e Integrated Gradients) resulta clave para aumentar la transparencia y confiabilidad de los modelos, especialmente en aplicaciones educativas o científicas.
 
 ## Evidencias
- Dataset: Oxford Flowers102 (~8000 imágenes, 102 clases).
- Modelo: EfficientNetB0 (base preentrenada, capa densa final softmax).
- Parámetros entrenables: Solo la capa final.
- Entrenamiento: 5 épocas con data augmentation.
- Resultados: Accuracy validación >85% (dependiendo del hardware).
- Archivo generado: mi_modelo_flores.h5.

## Reflexión personal

Este práctico me ayudó a visualizar cómo las técnicas de augmentation permiten simular condiciones reales sin recolectar más datos, fortaleciendo la generalización del modelo.
También entendí la importancia de la explicabilidad en contextos científicos, donde no basta con predecir bien: hay que entender por qué el modelo decide así.
El uso de transfer learning simplificó enormemente el proceso, mostrando cómo se pueden obtener resultados sólidos incluso con recursos limitados.

## Referencias
- TensorFlow Datasets - Oxford Flowers102
- Keras Applications Documentation
- Albumentations Library
- GradCAM Paper
- Integrated Gradients Paper
- Mixup: Beyond Empirical Risk Minimization
- CutMix: Regularization Strategy to Train Strong Classifiers