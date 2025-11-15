# Segment Anything (SAM) para Flood Segmentation – Práctica 12

## Contexto  
Esta práctica tuvo como finalidad aplicar **segmentación semántica** usando el modelo **Segment Anything (SAM)**, comparando su desempeño en modo **zero-shot** contra un modelo **fine-tuned específicamente para segmentación de inundaciones**.  
Se trabajó con el dataset **Flood Area Segmentation (Kaggle)**, compuesto por 290 imágenes satelitales con máscaras binarias de agua, en un escenario realista de monitoreo de desastres naturales.

El objetivo principal fue **evaluar la capacidad del modelo SAM preentrenado para segmentar áreas inundadas sin entrenamiento adicional**, y luego **medir la mejora obtenida al ajustar el decoder del modelo para este dominio específico**.

---

## Objetivos
- Evaluar la capacidad zero-shot de SAM usando prompts (points y boxes).  
- Entrenar un modelo fine-tuned para mejorar la segmentación de agua en imágenes satelitales.  
- Comparar métricas objetivas (IoU, Dice, Precision, Recall) entre ambos enfoques.  
- Analizar mejoras, limitaciones y posibles aplicaciones en entornos reales de emergencia.  

---

## Actividades Realizadas

1. **Carga y exploración del dataset Flood Area Segmentation**  
   - 290 imágenes RGB + máscaras binarias  
   - Inspección del ratio de área inundada  
   - Visualización conjunta imagen/máscara  

2. **Inferencia zero-shot con SAM ViT-B**  
   - Aplicación de **point prompts** y **box prompts**  
   - Cálculo de métricas por imagen  
   - Comparación de distribuciones IoU, Dice y Recall entre tipos de prompt  

3. **Arquitectura SAM y estrategia de fine-tuning**  
   - Congelación del Image Encoder (ViT) y Prompt Encoder  
   - Entrenamiento solo del Mask Decoder  

4. **Preparación del dataset para entrenamiento**  
   - Resize a 1024×1024  
   - Aumentaciones: Flip, Rotate, BrightnessContrast  

5. **Entrenamiento supervisado**  
   - Loss: BCE + Dice Loss  
   - Optimizer: Adam, lr = 1e-4 + StepLR scheduler  
   - ~10-20 epochs, batch_size=1-2  
   - Curvas de entrenamiento sin signos de overfitting  

6. **Evaluación y comparación final**  
   - Cálculo de métricas promedio  
   - Visualización lado a lado (GT vs pretrained vs fine-tuned)  
   - Análisis de errores reducidos tras el ajuste  

---

## Desarrollo y Resultados

### Zero-shot con SAM (Modelo Preentrenado)

El modelo mostró resultados moderados:

| Prompt | IoU Promedio | Observaciones |
|--------|--------------|---------------|
| Point | ~0.40 | Segmenta solo regiones evidentes, se pierde en bordes irregulares |
| Box | ~0.45 | Mejora leve, pero confunde reflejos y sombras |

Problemas detectados:

- Falsos positivos en ríos, caminos mojados o zonas de sombra  
- Segmentaciones incompletas en agua turbia o sin contraste  
- Se observa que el modelo fue entrenado para objetos cotidianos, **no para segmentar cuerpos de agua**  

---

### Fine-tuning del SAM

Solo se entrenó el decoder, dando mayor rapidez y menor requerimiento de VRAM.

| Modelo | IoU | Dice | Observación |
|--------|-----|------|-------------|
| Pretrained | ~0.45 | ~0.59 | Resultados poco consistentes |
| Fine-tuned | ~0.72 | ~0.83 | Segmentaciones mucho más precisas |

Mejoras observadas:

- Contornos más correctos en zonas parcialmente inundadas  
- Reducción de falsos positivos en sombras (>40%)  
- Más robusto ante variaciones de luz y color  

---

## Visualización Comparativa

- GT (Ground Truth) → máscara real
- SAM Pretrained → predicción incompleta y fragmentada
- SAM Fine-tuned → contornos ajustados a la topología real del agua

---

## Reflexión

Esta práctica permitió comprobar que **SAM es un modelo extremadamente poderoso, pero no infalible fuera de su dominio preentrenado**.  
En modo zero-shot, SAM se apoya fuertemente en patrones visuales generales y falla en:

- Inundaciones sin bordes claros  
- Agua con reflejos o baja saturación  
- Escenarios satelitales con ruido o poca estructura visual

El fine-tuning del **mask decoder** fue suficiente para lograr una mejora notable, sin necesidad de reentrenar el costoso encoder ViT. Esto demuestra que:

> *SAM no es una solución mágica universal, pero puede adaptarse con muy pocos datos y lograr resultados de estado del arte.*

Además, reforzó la importancia de:

- Evaluar con métricas como **IoU + Dice**, ya que una sola métrica puede ser engañosa  
- Entender cuándo priorizar **generalización (zero-shot)** vs **especialización (fine-tuning)**  
- Ajustar solo partes del modelo para maximizar beneficios computacionales

---

## Reflexión Personal

Este práctico me permitió profundizar en:

- La arquitectura modular de SAM  
- Cómo funcionan los **prompt-based segmentation models**  
- La diferencia entre *general foundation models* y *task-adapted models*  
- La importancia del **fine-tuning parcial** como compromiso entre costo y performance  

También comprendí que en tareas críticas como respuesta a emergencias, **no basta con un modelo genérico**, se requiere adaptar y evaluar exhaustivamente antes del deployment.

En general, fue una experiencia sumamente enriquecedora que integró segmentación semántica, transferencia de aprendizaje y aplicación práctica en escenarios reales de impacto social.

---

## Evidencias

- Notebook de entrenamiento y evaluación  
- Métricas registradas en tabla comparativa  
- Visualizaciones superpuestas (GT vs predicciones)  
- Curvas de loss e IoU por época  

---

## Referencias

- Meta AI – **Segment Anything**  
- Kaggle – Flood Area Segmentation Dataset  
- PyTorch Documentation  
- Albumentations (Data Augmentation)  
- scikit-image (Preprocessing Tools)  
- Kirillov et al., 2023 – Segment Anything Paper  
