# Proyecto Final: Búsqueda Semántica en Español

**Comparación de representaciones vectoriales de texto para recuperación de información**

Materia: Inteligencia Artificial
Autor: Alejandro Vásquez Sánchez ([@alejox1888](https://github.com/alejox1888))

---

## Descripción

Este proyecto construye y compara **cinco enfoques** para representar texto en español como vectores numéricos, evaluados sobre la tarea de **búsqueda semántica** (recuperación del documento más relevante para una pregunta dada).

La pregunta central del proyecto es: *¿cómo logramos que un buscador entienda el significado de lo que escribimos y no solo las palabras exactas?* Para responderla, se implementó el modelo **Word2Vec (Skip-gram con Negative Sampling) desde cero en PyTorch** sobre Wikipedia en español, siguiendo la formulación original de [Mikolov et al. (2013)](https://arxiv.org/pdf/1301.3781), y se comparó contra cuatro alternativas estándar (TF-IDF, Word2Vec de gensim, GloVe preentrenado y Sentence-Transformers) sobre el conjunto de evaluación **mMARCO en español**.

## Resultados principales

| Método | Precisión@1 | Precisión@5 | Precisión@10 | MRR |
|---|---|---|---|---|
| TF-IDF (referencia) | 54.0 % | 79.0 % | 82.5 % | 64.6 % |
| Word2Vec (PyTorch, propio) | 38.0 % | 60.5 % | 71.5 % | 49.7 % |
| Word2Vec (gensim) | 35.0 % | 62.0 % | 69.5 % | 46.6 % |
| GloVe (preentrenado) | 30.0 % | 49.5 % | 57.0 % | 39.0 % |
| **Sentence-Transformers** | **63.0 %** | **83.0 %** | **88.5 %** | **72.3 %** |

Más detalles en el [informe completo](report/Informe_Proyecto_Final_IA.pdf).

## Estructura del repositorio

```
Proyecto-Final-IA/
├── README.md                    # Este archivo
├── requirements.txt             # Dependencias de Python
├── AI_USAGE.md                  # Declaración del uso de herramientas de IA
├── notebooks/                   # Notebooks ejecutables (en orden)
│   ├── 01_corpus_preparation.ipynb       # Descarga y limpieza de Wikipedia ES
│   ├── 02_word2vec_from_scratch.ipynb    # Skip-gram desde cero en PyTorch
│   ├── 03_glove_and_sbert_loading.ipynb  # GloVe, gensim y Sentence-Transformers
│   ├── 04_search_evaluation.ipynb        # Buscador y métricas sobre mMARCO
│   └── 05_visualization_tsne.ipynb       # Visualizaciones t-SNE
├── report/
│   └── Informe_Proyecto_Final_IA.pdf     # Informe escrito (2 páginas)
└── slides/
    └── Diapositivas_Presentacion.pdf     # Diapositivas del video
```

## Cómo ejecutar el proyecto

El proyecto fue desarrollado en **Google Colab** con GPU Tesla T4. Para reproducirlo:

### Requisitos previos
- Una cuenta de Google (para Colab y Drive).
- Aproximadamente **3 GB de espacio libre en Google Drive** (para corpus, embeddings y archivos descargados).

### Pasos

1. **Clonar o descargar este repositorio** y subir la carpeta `notebooks/` a tu Google Drive.

2. **Crear una carpeta de trabajo** en tu Drive llamada `proyecto_ia_final/`. Los notebooks la usan como directorio base; ahí se guardarán los archivos intermedios.

3. **Abrir cada notebook desde Colab** en el orden numerado. Cada uno comienza con una celda que monta Google Drive y configura el directorio de trabajo automáticamente.

4. **Activar la GPU** en Colab para el notebook 2 (`Entorno de ejecución → Cambiar tipo de entorno → GPU T4`). Los demás notebooks pueden correr en CPU.

5. **Instalar dependencias** (la primera celda de cada notebook lo hace con `pip install`). Las versiones probadas están en `requirements.txt`.

### Orden de ejecución

| Notebook | Qué hace | Tiempo aprox. | Hardware |
|---|---|---|---|
| 01 | Descarga 50.000 artículos de Wikipedia ES, los limpia y tokeniza | 20-30 min | CPU |
| 02 | Entrena Word2Vec Skip-gram desde cero | ~4 horas | **GPU** |
| 03 | Entrena word2vec con gensim, carga GloVe (~1.6 GB) y SBERT | 15-30 min | CPU |
| 04 | Construye buscador, evalúa los 5 métodos sobre mMARCO | 20-30 min | CPU/GPU |
| 05 | Genera visualizaciones t-SNE | 10-15 min | CPU |

Los archivos intermedios (corpus tokenizado, vocabulario, embeddings entrenados, embeddings de GloVe) se guardan en `proyecto_ia_final/data/` dentro de Drive entre notebooks; no se incluyen en este repositorio porque algunos pesan más de 1 GB.

## Datos utilizados

- **Corpus de entrenamiento**: subconjunto de [Wikipedia en español](https://huggingface.co/datasets/wikimedia/wikipedia) (50.000 artículos, ~25 millones de palabras).
- **Embeddings preentrenados de GloVe**: del repositorio [spanish-word-embeddings de la Universidad de Chile](https://github.com/dccuchile/spanish-word-embeddings), entrenados sobre el Spanish Billion Words Corpus.
- **Modelo SBERT**: [`paraphrase-multilingual-MiniLM-L12-v2`](https://huggingface.co/sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2) de la librería sentence-transformers.
- **Evaluación**: [mMARCO en español](https://huggingface.co/datasets/unicamp-dl/mmarco), subconjunto de 5.000 documentos y 200 queries.

Ninguno de estos conjuntos se incluye directamente en el repositorio; los notebooks los descargan automáticamente.

## Modelo entrenado

El notebook 2 produce el archivo `word2vec_pytorch.npy` (~32 MB), la matriz de embeddings de palabras entrenada desde cero. No se incluye en el repositorio por tamaño; se regenera al ejecutar el notebook 2 (aproximadamente 4 horas en GPU T4).

Hiperparámetros del entrenamiento:
- Dimensión de los vectores: **100**
- Ventana de contexto: **5**
- Muestras negativas por par positivo: **5**
- Optimizador: **Adam**, tasa de aprendizaje **0.005**
- Tamaño de lote: **4.096**
- Épocas: **15**
- Función de pérdida: **entropía cruzada binaria** (negative sampling)

## Referencias

- Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781.
- Mikolov, T., Sutskever, I., Chen, K., Corrado, G., & Dean, J. (2013). *Distributed Representations of Words and Phrases and their Compositionality*. NeurIPS.
- Pennington, J., Socher, R., & Manning, C. D. (2014). *GloVe: Global Vectors for Word Representation*. EMNLP.
- Reimers, N., & Gurevych, I. (2019). *Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks*. EMNLP.
- Bonifacio, L. et al. (2021). *mMARCO: A Multilingual Version of the MS MARCO Passage Ranking Dataset*. arXiv:2108.13897.

## Uso de herramientas de IA

Ver [`AI_USAGE.md`](AI_USAGE.md) para la declaración detallada del uso de herramientas de inteligencia artificial durante el desarrollo de este proyecto.
