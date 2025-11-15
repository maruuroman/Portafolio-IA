# YOLOv8 Fine-tuning & Object Tracking – Práctica 11

## Contexto  
Esta práctica tuvo como finalidad aplicar los conceptos de **detección de objetos**, **fine-tuning con YOLOv8** y **tracking multi-objeto** en un caso de uso realista dentro del contexto de retail.  
Se trabajó con el dataset **Fruit Detection** (Kaggle), compuesto por imágenes de frutas anotadas con bounding boxes pertenecientes a 6 clases.  
El objetivo fue **comparar el desempeño entre el modelo generalista YOLOv8n preentrenado con COCO y un modelo fine-tuned específicamente para frutas**, evaluando métricas como mAP, Precision y Recall, y finalmente aplicar seguimiento persistente en video mediante **Norfair**.

---

## Objetivos
- Realizar inferencia base con YOLOv8 preentrenado para identificar sus limitaciones fuera de dominio.  
- Entrenar un modelo especializado mediante **fine-tuning** sobre un dataset de frutas.  
- Evaluar el impacto del entrenamiento en métricas cuantitativas y detecciones visuales.  
- Implementar un sistema de **tracking multi-objeto** sobre video usando Norfair.  
- Analizar resultados, errores y trade-offs del enfoque adoptado.

---

## Actividades Realizadas

1. **Setup de entorno** con Ultralytics, OpenCV, GPU enablement y carga del modelo base YOLOv8n.
2. **Descarga y validación del dataset Fruit Detection**, construcción del archivo `data.yaml` y verificación de clases (`apple, banana, grapes, orange, pineapple, watermelon`).
3. **Exploración del dataset**: conteo de instancias por clase, análisis de balance/desbalance y reflexión sobre su efecto en mAP.
4. **Visualización de bounding boxes** renderizando anotaciones desde archivos `.txt` en formato YOLO.
5. **Fine-tuning acelerado** del modelo base usando `EPOCHS=10`, `BATCH_SIZE=16`, `FRACTION=0.25`, guardando pesos y gráficos de entrenamiento.
6. **Carga del best.pt** generado durante el entrenamiento y comparación de clases respecto al modelo COCO.
7. **Evaluación cuantitativa** mediante `model.val()`, analizando mAP, Precision, Recall y métricas por clase.
8. **Comparación cualitativa** entre detecciones before vs after sobre imágenes reales de validación.
9. **Análisis de errores** mediante conteo de TP, FP, FN usando IoU ≥ 0.5.
10. **Tracking multi-objeto** con Norfair sobre un video de frutas, generando IDs persistentes y estadísticas sobre la duración de los tracks.

---

## Desarrollo y Resultados

### Inferencia Base (Modelo COCO)
El modelo preentrenado en COCO pudo detectar algunas frutas (como banana y orange) pero **falló en objetos menos frecuentes**, mostrando:

- Falsos negativos (frutas no detectadas)
- Falsas detecciones de clases no relevantes (p.ej. “sports ball”)
- Bounding boxes con baja confianza en escenas de góndolas

Esto evidenció la necesidad de especializar el modelo.

---

### El entrenamiento produjo:
- Reducción progresiva del box_loss y cls_loss
- mAP significativamente mayor respecto al modelo base
- Mejor detección de frutas pequeñas y parcialmente ocluidas

## Reflexión
Esta práctica permitió comprobar que YOLOv8 es  eficiente al combinar detección y entrenamiento especializado, y que el fine-tuning es clave cuando el modelo generalista no se adapta bien al dominio deseado.

El salto en mAP confirma que el conocimiento previo del modelo base puede refinarse con pocos epochs siempre que se cuenten con anotaciones de calidad.

La integración con Norfair permitió comprender cómo se extiende la detección hacia un escenario temporal, donde las decisiones deben persistir entre frames.

## Reflexión Personal
Esta práctica me ayudó a consolidar conocimientos sobre:
- La diferencia entre detección y clasificación
- El impacto real del fine-tuning en métricas y uso práctico
- Cómo interpretar correctamente mAP, Precision, Recall y IoU
- El papel de los FP/FN en el análisis de resultados
- El funcionamiento interno de un sistema de tracking multi-objeto y el efecto de parámetros como distance_threshold

También pude experimentar con decisiones prácticas como reducir el dataset mediante fraction=0.25 o usar un modelo liviano (nano) para acelerar el prototipado sin perder validez experimental.

En conjunto, fue una experiencia muy valiosa que integró visión por computadora, entrenamiento supervisado y análisis temporal, acercándose a escenarios reales de retail inteligente.

### Evidencias
- Entrenamiento y validación en Google Colab
- Pesos generados en runs/detect/fruit_finetuned/weights/best.pt
- Gráficos de entrenamiento (results.png)
- Video exportado con tracking persistente
- Notebook completo con ejecución paso a paso

### Referencias
- Ultralytics YOLOv8 Docs & Training Guide
- Fruit Detection Dataset – Kaggle
- Norfair – Multi Object Tracking
- SORT (Bewley et al., 2016)
- DeepSORT (Wojke et al., 2017)
- ByteTrack (2022)
