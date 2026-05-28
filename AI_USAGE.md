# Declaración de uso de herramientas de IA

Este proyecto utilizó **Claude (Anthropic)** como asistente durante su desarrollo. A continuación se detalla en qué partes y de qué manera.

## Herramienta utilizada

- **Claude (modelo de Anthropic)**, accedido a través de la interfaz web.

## Propósito y partes del proyecto donde se usó

| Componente | Uso de IA | Trabajo del estudiante |
|---|---|---|
| **Diseño del proyecto y alcance** | Discusión y propuestas alternativas; estimación de tiempos de cómputo. | Decisión final sobre el alcance, los modelos a comparar y la metodología. |
| **Notebooks (código)** | Generación inicial del código para los cinco notebooks. | Ejecución completa (incluyendo ~4 horas de entrenamiento en GPU), debugging de errores reales (problemas de memoria RAM, dataset deprecado en HuggingFace, URL de GloVe rota), validación de los outputs de cada celda. |
| **Resolución de problemas técnicos** | Diagnóstico y propuestas de solución (rediseño del Dataset por OOM, downgrade de `datasets<3.0` para mMARCO, URL alternativa de GloVe). | Identificación y reporte de los errores; aplicación y verificación de las correcciones. |
| **Informe escrito (2 páginas)** | Redacción del documento LaTeX a partir de los resultados experimentales. | Resultados experimentales reales; revisión y validación del contenido antes de la entrega. |
| **Diapositivas y guion del video** | Generación de la estructura, contenido y plantilla en Beamer. | Definición del alcance, exposición y defensa del proyecto en el video. |
| **Documentación (README y este archivo)** | Redacción inicial. | Revisión y aprobación final. |

## Validación y responsabilidad

Conforme a la política del curso, el estudiante asume plena responsabilidad por el trabajo entregado: comprende los conceptos teóricos detrás de cada modelo (Skip-gram con Negative Sampling, GloVe, Sentence-Transformers, TF-IDF), puede explicar las decisiones de diseño tomadas, interpreta los resultados obtenidos, y está en capacidad de defender la totalidad del contenido durante la presentación oral y ante cualquier pregunta del evaluador.

Todos los resultados numéricos, gráficos y análisis presentados provienen de la ejecución real de los notebooks por parte del estudiante en Google Colab; ningún resultado fue generado o simulado por la IA.
