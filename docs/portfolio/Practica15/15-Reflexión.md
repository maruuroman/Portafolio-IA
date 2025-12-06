# Agentes con LangGraph — RAG, Tools y Memoria Conversacional - Práctico 15
## Contexto

Los sistemas conversacionales modernos han evolucionado más allá de los chatbots simples hacia agentes inteligentes capaces de razonar, usar herramientas externas y mantener memoria de interacciones previas.

LangGraph permite modelar estos agentes como grafos de estado, donde cada nodo representa una habilidad (razonamiento LLM, búsqueda RAG, memoria, herramientas externas) y las transiciones definen flujos dinámicos y cíclicos.

Esta práctica desarrolla desde cero un agente multi-herramienta que integra:

- RAG con FAISS para fundamentar respuestas en documentación real
- Tool calling automático para ejecutar funciones externas (pedidos, hora UTC, búsqueda)
- Memoria conversacional dinámica mediante resúmenes periódicos
- UI en Gradio para interacción y debugging visual

El objetivo fue diseñar un agente capaz de decidir autónomamente cuándo usar sus herramientas, cómo combinar información y cuándo responder directamente, todo dentro de una arquitectura trazable y depurable.

## Objetivos

- Construir un AgentState robusto que almacene mensajes multi-turno y soporte resúmenes compactos.
- Diseñar un RAG reutilizable utilizando embeddings de OpenAI y FAISS como vector store local.
- Integrar tool calling dinámico para que el modelo decida entre RAG, consulta de pedidos o tiempo UTC.
- Diseñar un grafo cíclico en LangGraph con routing condicional assistant → tools → assistant.
- Desplegar una interfaz en Gradio con historial persistente y log de herramientas usadas.

## Actividades
Actividad	Descripción	Resultado Obtenido
1. Setup y Hello Agent	Instalación de langgraph, langchain-openai, faiss-cpu y creación del grafo mínimo START → assistant → END.	Validación completa del entorno. El agente inicial responde correctamente.
2. Estado con memoria	Definición de AgentState con messages y summary: Optional[str].	Estado preparado para acumular mensajes y comprimir memoria.
3. Construcción de RAG	Creación de vector store FAISS con textos sobre LangGraph y RAG. Embeddings OpenAI, retriever k=3.	Base indexada capaz de devolver los top chunks más relevantes.
4. Tool RAG	Uso de @tool para exponer rag_search(question) como herramienta accesible al LLM.	Tool integrada. Devuelve texto concatenado y legible.
5. Tools adicionales	Implementación de get_order_status y get_utc_time.	Tools externas funcionando para pruebas de tool calling.
6. LLM con tool binding	Enlazar tools al modelo gpt-4o-mini y crear ToolNode en LangGraph.	El LLM genera tool_calls correctamente.
7. Routing condicional	Función route_from_assistant() para decidir si ejecutar tools o finalizar.	Flujo dinámico entre assistant y tools.
8. Grafo assistant ↔ tools	Construcción del StateGraph con ciclo condicional assistant ⇄ tools.	Ciclo funcional donde el agente usa tools según lo necesite.
9. Conversación multi-turno	Pruebas con preguntas consecutivas reutilizando estado.	Primera pregunta responde directo, segunda usa RAG.
10. Streaming de eventos	Uso de graph.stream para observar el flujo interno.	Log con human → ai → tool → ai. Útil para debugging.
11. Nodo de memoria	Implementación de resúmenes automáticos en 3 bullets.	Summary dinámico: captura intenciones y temas previos.
12. Interfaz Gradio	Construcción de UI interactiva con logs de tools.	App funcional para pruebas externas.

## Desarrollo

Paso 1–2: Setup y Estado Extendido

**Decisiones técnicas:**

- langgraph>=0.2.0 e langchain-openai para modelo + herramientas
- faiss-cpu como motor vectorial liviano

AgentState con:

- messages acumulativos
- summary: Optional[str] para resúmenes periódicos

Componentes clave:

- START, END, y construcción del grafo lineal base
- gpt-4o-mini: balance entre velocidad y calidad
- assistant_node: ejecuta llm.invoke(state["messages"]) utilizando historial completo
Resultado: Un agente mínimo totalmente funcional con estado explícito.

## Reflexión
La elección de Optional[str] para summary ofrece flexibilidad: permite empezar sin resumen, actualizarlo cuando haga falta y mantener el contexto bajo control aunque la conversación crezca ilimitadamente.

Paso 3–5: RAG y Tools Adicionales

Pipeline RAG:
Documentos → TextSplitter → OpenAI Embeddings → FAISS VectorStore → Retriever(k=3)

Tool RAG:
@tool sobre rag_search(question)

Devuelve concatenado de los top-3 chunks
Sin JSON innecesario → mejor interpretación del modelo

Tools adicionales:

get_order_status(order_id) — simula sistema externo
get_utc_time() — retorna hora actual en formato ISO

Reflexión – Escalabilidad RAG:
Para corpus grandes (>50k documentos):

Vector stores distribuidos (Pinecone, Weaviate)
Filtros por metadata (tema, fecha)
Reranking con modelos cross-encoder
Embeddings económicos (embedding-3-small)

Paso 6–9: Grafo con Tool Calling y Multi-turno

Arquitectura:

START → assistant → ¿tool_calls?
              ├── Sí → tools → assistant (ciclo)
              └── No → END


Routing:

Si el AIMessage incluye tool_calls → nodo tools

Tools retornan resultados → assistant continúa con el análisis

Sin tool calls → conversación finaliza

Ejemplo real:

Usuario: “¿Qué es RAG?”

Assistant genera tool_call a rag_search

Nodo tools ejecuta FAISS

Assistant sintetiza usando contexto

Sin tool_calls → END

Reflexión — Escalar a 10+ tools:

Clasificación previa de intención

Descripciones claras de cada tool

Few-shots que muestren correctamente cuándo usarlas


Paso 10–11: Memoria Ligera con Summary

Mecanismo:

Se extraen los últimos 6 mensajes

El modelo resume en 3 bullets agregando el resumen previo

El nuevo summary sustituye al anterior

Ventajas:

Mantiene coherencia en conversaciones largas

Evita costos extremos por contexto creciente

Facilita persistencia y recuperación de sesiones

Reflexión:
El summary debe omitir PII o datos irrelevantes, y centrarse en:

Problemas del usuario

Preferencias

Decisiones tomadas

Información estructural


Paso 12: Interfaz Gradio

Componentes:

gr.Chatbot para historial

gr.State para AgentState persistente

gr.Markdown para log de tools

Callback integra input → grafo → output

## Resultado:
App publicada temporalmente:
https://3db1abf7ac1edf0043.gradio.live
Permite interacción sin necesidad de ejecutar el notebook.

## Reflexión

Esta práctica demuestra que LangGraph simplifica la construcción de agentes autónomos, transformando complejidad en una arquitectura clara y modular.

Las respuestas se vuelven inconsistentes

El costo de tokens aumenta drásticamente

La memoria dinámica ofrece rendimiento, coherencia y escalabilidad.

## Evidencias
- En el archivo [Practica15](15-Practica15.ipynb) se encuantran realizada la actividad.