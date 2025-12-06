---
title: "EDA del Titanic – Práctica 1"
date: 2025-01-01
---

# EDA del Titanic – Práctica 1

## Contexto
Exploración de datos (EDA) del clásico dataset del Titanic de Kaggle.  
El objetivo de la competencia es predecir la supervivencia de los pasajeros a partir de sus características (edad, sexo, clase, etc.).  
Esta práctica se realizó en Google Colab, utilizando Python y librerías de análisis de datos.

## Objetivos
- Comprender la estructura y calidad del dataset del Titanic.  
- Identificar variables clave asociadas a la supervivencia.  
- Practicar técnicas básicas de análisis exploratorio y visualización.

## Actividades (con tiempos estimados)
| Actividad                                  | Tiempo | Resultado esperado                                    |
|---------------------------------------------|:------:|-------------------------------------------------------|
| Investigación del dataset y la competencia  | 10 min | Contexto del problema y definición de variables        |
| Configuración de entorno en Google Colab    |  5 min | Librerías cargadas y carpeta de trabajo en Drive       |
| Descarga/carga de datos con Kaggle API      | 10 min | Archivos `train.csv` y `test.csv` disponibles          |
| Análisis exploratorio (shape, info, NA)     | 10 min | Vista general de columnas, tipos y valores faltantes   |
| Visualización de relaciones (Seaborn/Matplotlib) | 15 min | Gráficos de distribución, correlaciones y patrones     |

## Desarrollo
- **Investigación inicial:** Revisé la descripción del dataset en Kaggle para entender atributos y variable objetivo (`Survived`).  
- **Setup:** Configuré Google Colab, instalé librerías (`pandas`, `numpy`, `matplotlib`, `seaborn`), monté Google Drive y preparé carpetas de salida.  
- **Carga de datos:** Descargué `train.csv` y `test.csv` usando la API de Kaggle.  
- **Análisis básico:** Exploré dimensiones, encabezados, tipos de datos, estadísticos descriptivos y conteo de valores nulos.  
- **EDA visual:**  
  - Comparé supervivencia por **sexo**, confirmando mayor supervivencia de mujeres.  
  - Analicé supervivencia por **clase de pasajero (Pclass)**, observando ventaja en primera clase.  
  - Revisé distribución de **edad** frente a supervivencia.  
  - Calculé correlaciones numéricas (`Survived`, `Pclass`, `Age`, `SibSp`, `Parch`, `Fare`).

## Evidencias
- Se encuantran en el archivo "01-Práctica1.ipynb" dentro de esta carpeta. 
- Gráficos generados:
  - Conteo de supervivencia por sexo.
  - Tasa de supervivencia por clase.
  - Histograma de edad con supervivencia.
  - Mapa de calor de correlaciones.

## Reflexión
- **Aprendizajes:**  
  - La limpieza de datos es clave antes de modelar.  
  - Variables como sexo y clase tienen fuerte relación con la supervivencia.  
  - La visualización facilita descubrir patrones y guiar hipótesis.  
- **Mejoras futuras:**  
  - Imputar valores de edad de forma más robusta.  
  - Probar nuevas variables derivadas (familiares a bordo, títulos en el nombre).  
  - Preparar pipeline para modelado (por ejemplo, Logistic Regression o Random Forest).

## Referencias
- [Competencia Titanic en Kaggle](https://www.kaggle.com/competitions/titanic)
- Documentación de [pandas](https://pandas.pydata.org/), [seaborn](https://seaborn.pydata.org/) y [matplotlib](https://matplotlib.org/)