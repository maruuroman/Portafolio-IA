Introducción a Google Cloud y Qwiklabs – Práctica 1
Contexto

Esta práctica tuvo como finalidad introducir los conceptos básicos de Google Cloud Platform (GCP) y del entorno de aprendizaje Google Cloud Skills Boost, donde se ejecutan laboratorios prácticos mediante Qwiklabs.
Se trabajó directamente con el Cloud Console, explorando su interfaz, los servicios disponibles y la forma en que los laboratorios generan entornos temporales.
El objetivo fue comprender cómo funcionan los proyectos, los roles de IAM, la habilitación de APIs y la estructura general de un laboratorio, incluyendo el uso del panel de detalles y los mecanismos de seguimiento (scoring).

Objetivos

Acceder al Cloud Console utilizando credenciales temporales del laboratorio.

Identificar los componentes del entorno de Qwiklabs (Start Lab, créditos, tiempo, actividad).

Navegar por los proyectos disponibles y entender su estructura.

Revisar y modificar roles en Cloud IAM.

Explorar la biblioteca de APIs y habilitar servicios dentro de un proyecto.

Actividades Realizadas

Ingreso al laboratorio a través del botón Start Lab y revisión del entorno asignado.

Inicio de sesión en el Cloud Console con credenciales temporales provistas por Qwiklabs.

Exploración del panel de proyectos y visualización del proyecto principal y del proyecto Qwiklabs Resources.

Navegación por el menú principal para identificar servicios organizados por categorías.

Inspección de permisos actuales en IAM & Admin y asignación de un rol Viewer a un segundo usuario.

Acceso a APIs & Services y habilitación de una API específica (Dialogflow API).

Resolución de cuestionarios de “Test your understanding” incluidos en el laboratorio.

Desarrollo y Resultados
Exploración del Cloud Console

Se analizó la interfaz del Cloud Console, identificando sus elementos principales:

Menú de navegación lateral.

Panel superior de selección de proyectos.

Acceso a servicios como Compute, Storage, IAM, APIs, entre otros.

Se comprendió cómo los laboratorios generan un proyecto temporal, que queda activo únicamente durante el tiempo del contador. Además, se revisaron los elementos del panel del laboratorio:

Start Lab → Crea el entorno temporal
Credit → Costo del laboratorio
Time → Duración disponible
Score → Seguimiento de actividades

Gestión de Proyectos

Se observaron dos tipos de proyectos:

Proyecto temporal asignado, donde se realiza todo el trabajo.

Qwiklabs Resources, compartido en modo lectura con todos los estudiantes.

Se revisaron sus identificadores (nombre, número y Project ID) y el rol que cumplen al organizar recursos.

Roles y Permisos en IAM

En la consola se visualizó el usuario principal (estudiante) con rol Editor, que permite crear y modificar recursos.
Luego se otorgó el rol Viewer a un segundo usuario ficticio, aplicando:

IAM & Admin → Grant Access → Add principal → Select role → Viewer


Esto permitió comprender los roles básicos:

Viewer

Editor

Owner

APIs y Servicios

Finalmente, se exploró la API Library y se habilitó la Dialogflow API, verificando el proceso de activación de un servicio dentro de un proyecto temporal.

Reflexión

Esta práctica permitió comprender los fundamentos del ecosistema Google Cloud, especialmente el modo en que Qwiklabs gestiona entornos efímeros para aprendizaje.
Aprendí a identificar servicios desde el menú de navegación, gestionar roles básicos en IAM y habilitar APIs según sea necesario.
La experiencia fue clave para afianzar el uso del Cloud Console y entender cómo se estructura un proyecto en GCP.

Evidencias

Acceso al Cloud Console con credenciales temporales.

Revisión completa del panel del laboratorio (Start Lab, créditos, timer, score).

Asignación de roles desde IAM (Editor, Viewer).

Exploración de proyectos y visualización del proyecto “Qwiklabs Resources”.

Habilitación de Dialogflow API desde la API Library.

Resolución correcta de las preguntas de verificación del laboratorio.

Reflexión Personal

Esta práctica me ayudó a familiarizarme con los conceptos esenciales de Google Cloud, especialmente la organización por proyectos y el uso de IAM.
También reforzó mi capacidad para navegar eficientemente la consola y entender la lógica detrás de los entornos temporales de aprendizaje.
Fue una introducción muy clara y práctica que sienta las bases para trabajar con servicios más avanzados de la plataforma.

Referencias

Google Cloud Documentation (2024)

Cloud Console

IAM Basics

API Library

Google Cloud Skills Boost – Labs and Learning Paths

Qwiklabs Platform Guide