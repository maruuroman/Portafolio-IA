# LLMs con LangChain (OpenAI) — Prompting, Plantillas y Salida Estructurada (ES) - Practica 14

## Contexto
Esta práctica tuvo como finalidad experimentar con los componentes fundamentales de LangChain y su integración con modelos de OpenAI. Se trabajó con prompts, plantillas, salidas estructuradas y pipelines de RAG, evaluando su modularidad, control de formato y capacidad de fundamentar respuestas en documentos específicos.

El objetivo fue comparar enfoques zero-shot y few-shot, implementar structured output con Pydantic, y construir pipelines de recuperación + generación (RAG) para obtener respuestas confiables y consistentes.

## Objetivos

- Comprender la separación entre prompts, modelos y cadenas mediante LCEL (|).
- Obtener salidas estructuradas con Pydantic, eliminando parsing manual frágil.
- Experimentar con zero-shot y few-shot para clasificación y generación de texto.
- Implementar un pipeline RAG usando documentos locales.
- Analizar la importancia de observabilidad y trazas con LangSmith.
- Reflexionar sobre la modularidad y robustez de los flujos en LangChain.

## Actividades Realizadas

- Configuración del entorno Python con LangChain y OpenAI.
- Instalación de dependencias: langchain, langchain-openai, langsmith y otras opcionales.
- Creación de prompts y plantillas reutilizables mediante ChatPromptTemplate y operador | (LCEL).
- Configuración de modelos ChatOpenAI y ajuste de parámetros como temperature y max_tokens.
- Implementación de structured output con Pydantic para respuestas validadas en JSON.
- Experimentos comparando zero-shot vs few-shot para clasificación de texto.
- Construcción de un pipeline RAG con documentos locales y vector store FAISS.
- Evaluación de métricas, tokens y latencia usando LangSmith y callbacks.
- Reflexión sobre modularidad, consistencia y fundamentos de las respuestas.

## Desarrollo y Resultados
Prompts y LCEL

Se separaron instrucciones y contenido para mejorar reutilización:

from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_messages([
    ("system", "Sos un asistente conciso y exacto."),
    ("human", "Explicá {tema} en <=3 oraciones.")
])

llm = ChatOpenAI(model="gpt-5-mini", temperature=0)
chain = prompt | llm
print(chain.invoke({"tema": "atención multi-cabeza"}).content)


- Resultado: modularidad del código y control sobre la salida del LLM.

Structured Output con Pydantic

Se definieron esquemas para garantizar respuestas JSON válidas:

from pydantic import BaseModel
from typing import List

class Resumen(BaseModel):
    title: str
    bullets: List[str]

llm_json = llm.with_structured_output(Resumen)
res = llm_json.invoke("Resumí en 3 bullets los riesgos de prompt injection.")
res


- Resultado: eliminación de parsing manual frágil y mayor confiabilidad.

Zero-shot vs Few-shot

Se comparó desempeño y control de formato:

- Zero-shot: funciona bien para clasificación simple.
- Few-shot: mayor consistencia y control sobre la salida.

Pipeline RAG

Se implementó un pipeline simple con documentos locales:

from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_classic.chains import create_retrieval_chain

# Split y vector store
splitter = RecursiveCharacterTextSplitter(chunk_size=300, chunk_overlap=50)
chunks = splitter.split_documents(docs)
retriever = FAISS.from_documents(chunks, embedding=emb).as_retriever(search_kwargs={"k": 4})

rag_chain = create_retrieval_chain(retriever, combine_docs_chain)
rag_chain.invoke({"input": "¿Qué ventaja clave aporta RAG?"})


Resultado: respuestas fundamentadas en documentos específicos, demostrando el valor de RAG para grounding.

## Reflexión

Esta práctica permitió experimentar con los componentes fundamentales de LangChain:
- La separación entre prompts, modelos y cadenas mediante LCEL facilita la modularidad del código.
- El uso de structured output con Pydantic elimina la fragilidad del parsing manual y garantiza respuestas válidas.
- La comparación zero-shot vs few-shot mostró que ambos funcionan bien para clasificación simple, pero few-shot da mayor control sobre el formato.
- El pipeline RAG demostró cómo fundamentar respuestas en documentos específicos.
- Las advertencias de LangSmith evidencian la importancia de configurar correctamente las credenciales para observabilidad en producción.

## Evidencias

- Implementación de ChatPromptTemplate y flujos LCEL.
- Uso de with_structured_output para garantizar JSON válido.
- Experimentos con zero-shot y few-shot.
- Pipeline RAG con documentos locales y FAISS.
- Traza de tokens y latencia usando LangSmith.
- En el archivo [Practica14](14-Practica14.ipynb) se encuantran realizada la actividad.

## Reflexión Personal

Esta práctica me permitió entender la modularidad de LangChain y la importancia de la validación de salidas. Observé cómo few-shot mejora consistencia y cómo RAG aporta fundamentación a las respuestas. La experiencia reforzó mi comprensión sobre LLMs aplicados, estructuración de prompts y pipelines confiables, preparando el terreno para aplicaciones más complejas y seguras.

## Referencias
- LangChain Documentation 
- ChatPromptTemplate
- LCEL / Runnables
- Structured Output con Pydantic
- LangSmith / Tracing y Observabilidad
- RAG / Retrieval + Generation
- OpenAI API Documentation 
- Python / Pydantic Documentation 