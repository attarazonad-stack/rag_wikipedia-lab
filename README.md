# rag_wikipedia-lab
LangChain + ChromaDB + SentenceTransformers

Este proyecto implementa un sistema de retrieval-augmented generation (RAG) utilizando datos de Wikipedia para generar un resumen factual mediante recuperación vectorial.

🎯 Objetivo

Construir un pipeline RAG usando herramientas open-source.

Extraer, procesar y almacenar texto de Wikipedia.

Generar un resumen factual basado en retrieval.

Comparar el enfoque RAG con el enfoque multiagente (Tarea 1).

⚙️ Pasos del Proyecto
0️⃣ Configuración del entorno

Instalar dependencias:

pip install wikipedia-api sentence-transformers chromadb langchain langchain-community
pip install transformers torch pandas numpy markdown

1️⃣ Creación de conjuntos de datos

Extraer datos desde Wikipedia usando:

import wikipediaapi
wiki = wikipediaapi.Wikipedia('en')
page = wiki.page("Federated_learning")


Procesos:

Extraer el texto principal.

Dividirlo en fragmentos de ~300 palabras.

Guardarlo en:

/data/wiki_corpus.csv


Columnas:
id, title, text

2️⃣ Embeddings + Almacén vectorial (ChromaDB)

Crear embeddings e insertar:

from sentence_transformers import SentenceTransformer
import chromadb

model = SentenceTransformer("all-MiniLM-L6-v2")
client = chromadb.Client()
collection = client.create_collection("wiki_ai")


Insertar todos los fragmentos con:

texto

metadatos

embeddings generados

3️⃣ Canalización de consultas (LangChain + Ollama)

Crear la cadena RAG:

from langchain.chains import RetrievalQA
from langchain.llms import Ollama
from langchain.vectorstores import Chroma


Ejemplo:

qa = RetrievalQA.from_chain_type(
    llm=Ollama(model="mistral"),
    chain_type="stuff",
    retriever=Chroma(...).as_retriever()
)

qa.run("Explain federated learning challenges in healthcare.")

4️⃣ Generar y guardar resumen

A partir de los mejores fragmentos recuperados, generar un resumen coherente de 400–500 palabras y guardarlo como:

/outputs/rag_summary.md
