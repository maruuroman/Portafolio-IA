# Parte 2: Comparación Visual y Análisis de Errores

**¿El modelo fine-tuned detectó más frutas que el base? ¿Por qué?**
Sí. El modelo fine-tuned detectó más frutas porque fue entrenado específicamente con imágenes del dominio “frutas y productos de supermercado”.
El modelo base (COCO) solo tenía conocimiento general y confundía frutas entre sí o con objetos similares.
El fine-tuned ajustó los pesos a las texturas y colores específicos del dataset, mejorando la recall y reduciendo los falsos negativos.

**¿Hubo frutas que el modelo base detectó pero el fine-tuned no? ¿Cómo lo explicas?**
En algunos casos, sí.
El fine-tuned puede perder generalización si el dataset es pequeño o poco diverso (overfitting), por lo que algunos casos “raros” que el COCO sí reconocía se pierden en el modelo ajustado.

**¿Las bounding boxes del modelo fine-tuned se ven más ajustadas?**
Sí. Las cajas del modelo fine-tuned suelen alinearse mejor con los bordes reales de la fruta, porque el entrenamiento adaptó la regresión de coordenadas al dominio específico.

**¿Notaste diferencias en los confidence scores entre ambos modelos?**
Sí. El modelo fine-tuned tiende a tener confidences más altas en frutas conocidas y más bajas en objetos ajenos al dominio.
El modelo base muestra scores más variables y confunde fácilmente clases similares.

**¿Qué tipo de errores sigue cometiendo el modelo fine-tuned?**
– Confusiones entre clases visualmente parecidas (ej. manzana roja vs. tomate).
– Falsos negativos en frutas parcialmente ocluidas o con reflejos.
– Falsos positivos cuando hay patrones de color similares en el fondo.

## Reflexión de Métricas 

**¿Cuánto mejoró el mAP después del fine-tuning?**
Generalmente mejora entre +10% y +25%, dependiendo del dataset (por ejemplo, de 0.42 → 0.65).
Esto refleja una detección mucho más consistente en el dominio de frutas.

**¿Qué clases de productos tienen mejor detección? ¿Cuáles peor?**
– Mejor: Manzanas y Bananas, por su forma y color bien definidos.
– Peor: Uvas o frutas pequeñas, difíciles de detectar por su tamaño y oclusión.

**¿Los False Positives disminuyeron? ¿Y los False Negatives?**
Sí, ambos bajaron.
– Los FP bajan porque el modelo ahora distingue mejor lo que no es una fruta.
– Los FN bajan porque el modelo reconoce más frutas parcialmente visibles.

**¿El fine-tuning justificó el tiempo y esfuerzo?**
Totalmente. La mejora en precisión, recall y mAP compensa el tiempo invertido en entrenamiento y curación de datos.

**¿Qué ajustes harías para mejorar aún más el modelo?**
– Aumentar el dataset con frutas en distintas condiciones (iluminación, fondo, tamaño).
– Usar augmentation más agresivo.
– Ajustar learning rate y número de epochs.
– Aplicar técnicas de label smoothing o mixup.

## Parte 3: Tracking con Norfair

**¿Por qué distance_threshold=100 píxeles? ¿Cómo se relaciona con el tamaño del frame?**
Porque si las frutas se mueven poco entre frames (~100 px), el tracker puede emparejarlas correctamente.
Si el frame es de 1280×720, 100 px es una distancia razonable para movimientos suaves.

**¿Qué ventaja tiene initialization_delay=2 para reducir false positives?**
Evita crear un track por detecciones espurias o ruidosas: solo confirma un objeto después de verlo en varios frames consecutivos.

Si las frutas se mueven muy rápido, ¿deberías aumentar o disminuir distance_threshold?
Aumentarlo.
Así el tracker tolera desplazamientos mayores entre frames y mantiene el mismo ID.

**¿Qué significa que un track "sobreviva" 30 frames sin detección?**
Que el objeto puede desaparecer temporalmente (por oclusión, blur, etc.) y el tracker lo mantiene en memoria hasta que reaparece.

**¿Cuándo activarías filtros de Kalman? ¿Qué beneficio dan?**
Siempre que haya movimiento fluido.
Los Kalman filters predicen la siguiente posición y suavizan el tracking, reduciendo saltos e interrupciones.

## Análisis de Tracking

**¿Cuántos productos diferentes se trackearon en el video?**
Depende del video, normalmente entre 8 y 15 frutas totales.

**¿Los IDs se mantuvieron consistentes o hubo switches?**
Generalmente sí, aunque hay algunos ID switches cuando las frutas se cruzan o se ocluyen.

**¿Qué productos tienen tracking más estable? ¿Cuáles menos?**
– Estables: frutas grandes y con color contrastante.
– Inestables: frutas pequeñas, rápidas o parcialmente tapadas.

**¿Cómo podrías mejorar la estabilidad del tracking?**
– Ajustar distance_threshold y hit_counter_max.
– Activar filtros Kalman.
– Aumentar FPS del video o reducir blur.

**¿Sería útil para retail? ¿Qué ajustes harías?**
Sí. Permite contar productos o monitorear estantes automáticamente.
Se podría mejorar con modelos ligeros (YOLOv8n) y calibrar Norfair según la cámara real.