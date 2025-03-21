---
{"dg-publish":true,"permalink":"/procesamiento-de-lenguaje-natural-pln/","dgPassFrontmatter":true}
---

# 🧠 **Generalidades**

## 🚀 **¿Qué es el PLN?**
El **Procesamiento de Lenguaje Natural (PLN)** (en inglés, **Natural Language Processing - NLP**) es un campo de la inteligencia artificial (IA) que permite a las máquinas **comprender, interpretar, generar y responder** en lenguaje humano de manera natural y significativa.

El PLN combina conceptos de:  
✅ **Lingüística computacional** (estructura del lenguaje)  
✅ **Machine Learning** (aprendizaje a partir de datos)  
✅ **Deep Learning** (redes neuronales profundas)  

---

## 🎯 **Objetivo del PLN**
El objetivo principal es hacer que las computadoras:  
- Comprendan el lenguaje humano (texto y voz)  
- Generen respuestas coherentes  
- Interactúen de manera natural con las personas  

---

## 📌 **Componentes Clave del PLN**
### 1. **Tokenización**
Dividir el texto en unidades más pequeñas (**tokens**).  
**Ejemplo:**  
"El gato está en el sofá." → ["El", "gato", "está", "en", "el", "sofá", "."]  

---

### 2. **Lematización y Stemming**
- **Stemming:** Reduce las palabras a su raíz o forma base.  
  - "Jugando" → "Jug"  
- **Lematización:** Reduce las palabras a su forma base teniendo en cuenta el contexto gramatical.  
  - "Jugando" → "Jugar"  

---

### 3. **Etiquetado gramatical (POS Tagging)**
Identifica la categoría gramatical de cada token.  
**Ejemplo:**  
- "El" → artículo  
- "gato" → sustantivo  
- "está" → verbo  

---

### 4. **Reconocimiento de entidades (NER)**  
Identifica nombres propios, fechas, organizaciones, etc.  
**Ejemplo:**  
*"Apple anunció el nuevo iPhone el 12 de septiembre."*  
- Apple → Organización  
- iPhone → Producto  
- 12 de septiembre → Fecha  

---

### 5. **Análisis de dependencias (Dependency Parsing)**  
Analiza la estructura gramatical de la oración para determinar qué palabra depende de cuál.  

**Ejemplo:**  
*"El gato está en el sofá."*  
- "está" → verbo principal  
- "gato" → sujeto  
- "sofá" → objeto  

---

### 6. **Análisis de sentimiento**  
Evalúa el tono emocional del texto.  
- "¡Me encanta este producto!" → Sentimiento positivo  
- "El servicio fue terrible." → Sentimiento negativo  

---

### 7. **Generación de texto**  
Modelos como **GPT** o **BERT** pueden generar texto coherente en respuesta a una entrada específica.  
**Ejemplo:**  
Entrada → "¿Cuál es la capital de Francia?"  
Salida → "La capital de Francia es París."  

---

## 🔎 **Embeddings**
Un **embedding** es una representación vectorial de una palabra o token en un espacio multidimensional.  
- Los embeddings permiten que el modelo capture relaciones semánticas y sintácticas entre palabras.  
- Palabras con significados similares tendrán embeddings cercanos en el espacio vectorial.  

### ✅ **Ejemplo:**  
- "Rey" estará cerca de "reina" y "príncipe" en el espacio vectorial.  
- La relación "rey - hombre + mujer ≈ reina" funciona porque las palabras están organizadas por significado en el espacio vectorial.  

---

## 📌 **Tipos de embeddings**
### ✅ **Word2Vec** (2013)  
- Usa un modelo de redes neuronales para mapear palabras en un espacio vectorial.  
- Capta relaciones semánticas básicas ("rey" → "reina").  

### ✅ **GloVe** (2014)  
- Usa la co-ocurrencia de palabras en un corpus grande para construir embeddings.  
- Representación global del lenguaje.  

### ✅ **FastText** (2016)  
- Similar a Word2Vec, pero añade subpalabras (permite capturar significado de palabras desconocidas).  

### ✅ **Embeddings Contextuales (BERT, GPT)**  
- Los embeddings varían según el contexto de la frase.  
- "Banco" puede tener embeddings distintos según si se refiere a una institución financiera o a un objeto físico para sentarse.  

---

## 🧠 **Mecanismo de Atención**
El mecanismo de atención permite que el modelo analice las relaciones entre palabras dentro de una oración para modificar los embeddings y reflejar el contexto.

### ✅ ¿Cómo funciona?  
1. Cada token genera tres vectores:  
   - **Q (Query):** Lo que busca el token.  
   - **K (Key):** Información ofrecida por el token.  
   - **V (Value):** El valor que aporta el token al significado final.  

2. La atención calcula una combinación ponderada usando:  
$$
\text{Attention} = \text{softmax} \left( \frac{Q K^T}{\sqrt{d_k}} \right) V
$$

3. El resultado modifica los valores de las coordenadas del embedding para reflejar el contexto.

---

### 🔥 **Ejemplo de Atención:**
*"El gato está en el sofá."*  
- La atención permite que "está" se asocie con "gato" y "sofá" para captar el contexto de que un gato está en un sofá.  
- El embedding de "está" cambia para reflejar el sujeto ("gato") y el objeto ("sofá").  

---

## 📌 **Magnitud vs Dirección**
- El mecanismo de atención **no cambia directamente la magnitud** del vector, pero puede afectarla indirectamente.  
- El foco principal está en **cambiar la dirección** (es decir, las coordenadas) del vector para reflejar el contexto.  

---

## 📌 **¿Las Dimensiones del Embedding Representan Significado?**
- Las dimensiones **no tienen significados explícitos** como "género", "animal", "acción".  
- Sin embargo, las relaciones emergen durante el entrenamiento:  
    - "Rey - hombre + mujer ≈ reina"  
- El modelo organiza automáticamente el espacio vectorial para que estas relaciones se reflejen de manera latente.  

---

## 🌍 **Aplicaciones del PLN**
✅ **Traducción automática** → Google Translate  
✅ **Chatbots** → ChatGPT  
✅ **Resumen automático** → Resumir noticias  
✅ **Análisis de sentimiento** → Evaluar comentarios de clientes  
✅ **Reconocimiento de voz** → Siri, Alexa  
✅ **Generación de texto** → GPT, BERT  
✅ **Búsqueda semántica** → Google Search  

---

## 🚨 **Desafíos del PLN**
🚨 **Ambigüedad:** La misma palabra puede tener múltiples significados.  
🚨 **Contexto:** Comprender el contexto completo de una conversación o texto largo.  
🚨 **Ironía y sarcasmo:** Difíciles de captar para las máquinas.  
🚨 **Lenguaje informal:** Slang, emojis y lenguaje coloquial.  

---

## 🚀 **Resumen Clave**
✅ El texto → tokens → embeddings iniciales  
✅ El mecanismo de atención cambia las coordenadas (no solo la magnitud)  
✅ Los embeddings modificados reflejan el contexto semántico  
✅ Las relaciones complejas emergen en el espacio vectorial sin que las dimensiones estén definidas manualmente  

---

