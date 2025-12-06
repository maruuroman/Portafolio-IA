# Fine-tuning de Transformers para Clasificación Ofensiva - Práctico 13

## Contexto

Esta práctica tuvo como finalidad aplicar técnicas de procesamiento de lenguaje natural (NLP) enfocadas en el análisis de sentimiento financiero utilizando tanto métodos clásicos como modelos modernos basados en Transformers.

El trabajo se realizó sobre el dataset zeroshot/twitter-financial-news-sentiment, compuesto por tweets financieros clasificados en tres categorías: Bearish, Bullish y Neutral.
El objetivo fue comparar el desempeño entre un enfoque tradicional (TF-IDF + Regresión Logística) y un modelo Transformer especializado (FinBERT) mediante fine-tuning, evaluando cuál ofrece mejor capacidad de generalización en un entorno con alto desbalance de clases, ruido textual y vocabulario financiero especializado.

## Objetivos

- Cargar y preprocesar texto financiero utilizando datasets de Hugging Face.
- Implementar un baseline clásico utilizando TF-IDF + Logistic Regression.
- Aplicar fine-tuning a un modelo FinBERT adaptado al dominio financiero.
- Evaluar ambos modelos utilizando métricas adecuadas para datasets desbalanceados (macro-F1, matriz de confusión).
- Comparar el desempeño, costo computacional e interpretabilidad entre enfoques clásicos y modelos Transformer.
- Reflexionar sobre la viabilidad de cada enfoque en entornos reales de trading algorítmico.

## Actividades Realizadas

- Setup del entorno y carga del dataset financiero desde Hugging Face.
- Exploración inicial del dataset: distribución de longitudes y proporciones de clases.
- Análisis lexical mediante n-grams y WordClouds por clase.
- EDA avanzado: TF-IDF + PCA/UMAP y exploración de embeddings Word2Vec.
- Implementación del baseline TF-IDF (ngrams 1-2) + Logistic Regression.
- Entrenamiento del modelo FinBERT con fine-tuning y regularización.
- Comparación de métricas clave: accuracy, macro-F1 y matriz de confusión.
- Análisis de curvas de aprendizaje y detección de overfitting.
- Evaluación de trade-offs para despliegue en sistemas financieros reales.

## Desarrollo y Resultados
Baseline Clásico: TF-IDF + Logistic Regression

El pipeline clásico se construyó utilizando representación TF-IDF con max_features=10000 y n-grams (1,2), acompañado de una Logistic Regression con max_iter=1000 y split estratificado 80/20.

## Resultados principales:

Métrica	    Valor  <br>
Accuracy	~0.80  <br>
Macro F1	~0.69  <br>
Weighted F1	~0.78  

El modelo mostró un fuerte sesgo hacia la clase Neutral, reflejado en un recall del 97% para esta clase, pero con amplio margen de error en Bearish y Bullish.

Este desempeño confirma las limitaciones del enfoque Bag-of-Words en contextos con ruido, escasa separabilidad lineal y dependencia contextual entre tokens.

**Modelo Transformer: Fine-Tuning con FinBERT**

Se utilizó el modelo FinBERT (ProsusAI), especializado en texto financiero. El fine-tuning se realizó con:
learning_rate = 2e-5  <br>
batch_size = 16  <br>
num_train_epochs = 3  <br>
weight_decay = 0.01

Se empleó macro-F1 como métrica principal debido al desbalance del dataset.

# Resultados obtenidos:

| Modelo                       | Accuracy Test | Macro F1           |
|---------------------------------------------|:------:|-------------------------------------------------------|
| TF-IDF + Logistic Regression | ~0.80 | ~0.69 |
| FinBERT (fine-tuning)        | ~0.87 | ~0.83 |

FinBERT superó ampliamente al baseline, logrando mejoras del +7% en accuracy y +20% en macro-F1, especialmente en clases minoritarias.
Se observó una ligera señal de overfitting en la tercera época (incremento del validation loss), siendo el mejor rendimiento el de la época 2.

## Reflexión

La práctica permitió comprobar que los métodos clásicos como TF-IDF + Logistic Regression son rápidos, simples y útiles como baseline, pero su desempeño es limitado en datasets ruidosos y con vocabulario específico.

Por otro lado, el uso de Transformers como FinBERT mostró una superioridad evidente gracias a su comprensión contextual y su entrenamiento previo en textos financieros.

Esto implica que:
En entornos reales de mercado, donde las señales son sutiles y el lenguaje es altamente contextual, los Transformers ofrecen una ventaja significativa.

El costo computacional es mayor, pero puede mitigarse con técnicas como quantization, caching de embeddings y reducción de batch size.

Los modelos clásicos siguen siendo útiles en prototipos, validaciones rápidas y sistemas con restricciones de latencia estrictas.

## Evidencias

- Carga y preprocesamiento del dataset con Hugging Face datasets.
- WordClouds, análisis de n-grams y visualizaciones PCA/UMAP.
- Implementación de TF-IDF + LogisticRegression como baseline.
- Entrenamiento completo del modelo FinBERT con métricas por época.
- Matrices de confusión comparativas entre ambos modelos.
- Curvas de aprendizaje y evolución de accuracy/F1.
- En el archivo [Practica13](13-Practica13.ipynb) se encuantran realizada la actividad.

## Reflexión Personal

Esta práctica me permitió comprender en profundidad las diferencias entre enfoques clásicos de NLP y modelos modernos basados en Transformers.
Pude observar cómo los métodos tradicionales ofrecen simplicidad, pero quedan rápidamente limitados en dominios complejos como finanzas.

Vi de forma práctica cómo los modelos preentrenados aprovechan conocimiento contextual acumulado. Pude analizar cómo las métricas y las curvas de aprendizaje reflejan el impacto del desbalance de clases.

Aprendí a identificar señales de overfitting y a seleccionar la mejor época de entrenamiento.

En general, fue una experiencia que consolidó mis conocimientos sobre NLP aplicado al análisis financiero, así como la importancia de elegir el modelo adecuado según los requerimientos del problema.

## Referencias
- Hugging Face – Datasets & Transformers Documentation (2024)
- ProsusAI – FinBERT Model Card
- Scikit-learn Documentation – TF-IDF, Logistic Regression
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press.
- Jurafsky, D., & Martin, J. (2020). Speech and Language Processing.