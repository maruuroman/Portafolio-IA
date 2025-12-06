# Vertex AI Pipelines con Kubeflow – Práctica 17

## Contexto

Esta práctica tuvo como finalidad aprender a crear, ejecutar y gestionar pipelines de Machine Learning utilizando Vertex AI Pipelines en Google Cloud.
Vertex AI unifica distintos servicios de ML (AutoML, modelos personalizados, MLOps) dentro de una única API, facilitando el desarrollo, automatización y reproducibilidad de workflows.

Se trabajó con el entorno de Vertex AI Workbench, el SDK de Kubeflow Pipelines (KFP) y los componentes preconstruidos de Google Cloud.
El objetivo fue comprender por qué los pipelines son esenciales, crear un pipeline introductorio y luego construir un pipeline completo que entrena, evalúa y despliega un modelo AutoML.

## Objetivos

- Comprender el rol de los pipelines para automatizar, escalar y compartir workflows de ML.
- Instalar y utilizar el SDK de Kubeflow Pipelines.
- Crear y ejecutar un pipeline simple de 3 pasos usando componentes personalizados.
- Construir un pipeline que entrene, evalúe y despliegue un modelo AutoML de clasificación.
- Utilizar componentes preconstruidos del paquete google_cloud_pipeline_components.
- Programar la ejecución de un pipeline utilizando Cloud Scheduler.
- Entender cómo los pipelines permiten reproducibilidad y gestión de artefactos.

## Actividades Realizadas

- Inicio del entorno en Vertex AI Workbench y apertura de JupyterLab.
- Instalación del SDK de KFP y Google Cloud Pipeline Components.
- Configuración del proyecto, bucket y constantes (PROJECT_ID, REGION, PIPELINE_ROOT).
- Creación de componentes personalizados utilizando el decorador @component.
- Construcción de un pipeline introductorio compuesto por tres pasos (product_name, emoji, build_sentence).
- Compilación del pipeline a un archivo JSON mediante compiler.Compiler().compile().
- Ejecución del pipeline con AIPlatformClient.
- Inicio del pipeline completo para entrenar un modelo AutoML utilizando el dataset Dry Beans.
- Revisión del seguimiento del pipeline desde la interfaz de Vertex AI.

## Desarrollo y Resultados
Pipeline Introductorio  <br>

Se crearon tres componentes en KFP:  <br>
@component(base_image="python:3.12")  <br>
    def product_name(text: str) -> str: ...  <br>

@component(base_image="python:3.12", packages_to_install=["emoji"])  <br>
    def emoji(text: str) -> NamedTuple(...): ...  <br>

@component(base_image="python:3.12")  <br>
    def build_sentence(product: str, emoji: str, emojitext: str) -> str: ...  <br>

Estos se integraron en un pipeline usando @dsl.pipeline, generando una oración final construida a partir de las salidas previas.  <br>

El pipeline se compiló:  <br>

compiler.Compiler().compile(  <br>
    pipeline_func=intro_pipeline,  <br>
    package_path="intro_pipeline_job.json"  <br>
)  <br>

Y luego se ejecutó:  <br>
   api_client.create_run_from_job_spec("intro_pipeline_job.json")  <br>

El pipeline se ejecutó exitosamente y fue posible visualizar cada paso, su contenedor, entradas, salidas y logs desde Vertex AI Pipelines.

**Pipeline de Entrenamiento AutoML (end-to-end)**

Se inició la creación de un pipeline mayor que:
- Carga el dataset Dry Beans
- Entrena un modelo AutoML Tabular
- Evalúa el modelo
- Lo despliega en Vertex AI
- Este pipeline utiliza componentes preconstruidos del paquete:  <br>
    from google_cloud_pipeline_components import aiplatform as gcc_aip  <br>

El pipeline completo puede tardar más de 2 horas, por lo que solo se inició su ejecución dentro del laboratorio.

## Reflexión

Esta práctica permitió comprobar la importancia de los pipelines en el ciclo de vida de Machine Learning:
- Facilitan la automatización de procesos complejos.
- Permiten reproducibilidad total, gracias al uso de contenedores independientes por paso.
- Escalan fácilmente y pueden ser compartidos entre equipos.
- Integran servicios como AutoML, entrenamiento personalizado, almacenamiento de artefactos y despliegue.
- Mejoran la trazabilidad, ya que cada input/output queda registrado.
- Además, trabajar con el SDK de KFP reforzó la comprensión de cómo Vertex AI implementa MLOps a gran escala.

## Evidencias
- Archivos YAML generados de cada componente del pipeline.
- Archivo compilado intro_pipeline_job.json.
- Ejecución visible en Vertex AI Pipelines.
- Configuración de PROJECT_ID, BUCKET_NAME y PIPELINE_ROOT.
- Código para componentes personalizados y pipeline introductorio.
- Inicio del pipeline de entrenamiento AutoML en Vertex AI.

## Reflexión Personal

Esta práctica me permitió afianzar los conceptos de MLOps, especialmente la importancia de estructurar workflows mediante pipelines.
Comprendí mejor cómo cada paso del pipeline debe ser independiente, reproducible y fácilmente integrable en un entorno de producción.
El uso de componentes preconstruidos facilita mucho la interacción con servicios de Vertex AI, y la posibilidad de automatizar ejecuciones mediante Cloud Scheduler abre puertas a pipelines completamente desatendidos.

En general, fue una experiencia muy enriquecedora que reforzó mis conocimientos sobre pipelines, despliegue y automatización en entornos de Machine Learning.

## Referencias
- Vertex AI Documentation (2024)
- Vertex AI Pipelines
- Kubeflow Pipelines SDK
- AutoML Tabular
- Google Cloud Pipeline Components Library
- KOKLU, M. & OZKAN, I.A. (2020) — Dry Beans Dataset.
- Google Cloud Skills Boost – Vertex AI Pipelines Labs