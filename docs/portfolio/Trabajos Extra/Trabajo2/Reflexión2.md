---
title: "CIFAR-10 con CNN: del baseline MLP a un modelo con aumento de datos"
---

## Objetivo. 
Construir una CNN pequeña para CIFAR-10, usando data augmentation y early stopping, y comparar conceptualmente contra el baseline anterior con MLP (sin convoluciones).

## Datos y preparación
- Dataset: `tf.keras.datasets.cifar10` (10 clases de imágenes 32×32).
- Normalización en `[0,1]`.
- `class_names` para rotular predicciones.

## Modelo
- Arquitectura ligera: `Conv(32) → MaxPool → Conv(64) → MaxPool → Conv(128) → GAP → Dense(128) → Softmax`.
- Aumento de datos: `RandomFlip`, `RandomRotation`, `RandomZoom`.
- Optimizador: `Adam(1e-3)`; pérdida: `sparse_categorical_crossentropy`.

## Entrenamiento y evaluación
- `validation_split=0.1`, `batch_size=64`, hasta 40 épocas con EarlyStopping + ReduceLROnPlateau.
- Métrica principal: accuracy (train/val/test).
- Visualización: curvas de `accuracy` y `loss` por época y predicciones de ejemplo.

## Hallazgos (resumen)
- La CNN suele superar al MLP en CIFAR-10 gracias a invariancias y extracción local de patrones.
- Data augmentation reduce overfitting; early stopping evita entrenar de más.

## Próximos pasos
- Regularización adicional (Dropout/Weight Decay).
- Arquitecturas más profundas (ResNet pequeñas).
- *Mixup/Cutout* y *learning rate schedules*.
