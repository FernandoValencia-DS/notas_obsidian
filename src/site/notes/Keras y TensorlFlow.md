---
{"dg-publish":true,"permalink":"/keras-y-tensorl-flow/","dgPassFrontmatter":true}
---

Keras es una **biblioteca de alto nivel para el desarrollo de redes neuronales**. Está diseñada para facilitar la construcción, entrenamiento y evaluación de modelos de Machine Learning, especialmente redes profundas. Este se integra con TensorFlow que es el que se encarga de hacer el procesamiento. En pocas palabras Keras es un puente que le brinda al usuario una codificación más sencilla, luego eso lo lleva a TensorFlow que va a realizar el procesamiento de los datos.

Estas bibliotecas son ampliamente usadas para el desarrollo y entrenamiento de [[Redes Neuronales\|Redes Neuronales]] 

# Funciones

Crea un modelo secuencial, uno de los modelos más sencillos, compuesto de una única pila de capas conectadas de manera secuencial.

*model = keras.models.Sequential()*

Añade una capa con 300 neuronas y la función de activación ReLU, que vimos en [[Redes Neuronales\|Redes Neuronales]]. Nota: Podemos combinar diferentes funciones de activación en diferentes capas de una red dependiendo del objetivo.

*model.add(keras.layers.Dense(300, activation="relu"))*

## Ejemplo completo

Este ejemplo añade las capas una a una con *model.add*
*model = keras.models.Sequential()*
*model.add(keras.layers.Flatten(input_shape=[28, 28]))* 
*model.add(keras.layers.Dense(300, activation="relu"))*
*model.add(keras.layers.Dense(100, activation="relu"))*
*model.add(keras.layers.Dense(10, activation="softmax"))*

Como alternativa podemos incluir todo en un código único, así:
*model = keras.models.Sequential([ 
	keras.layers.Flatten(input_shape=[28, 28]), keras.layers.Dense(300, activation="relu"), 
	keras.layers.Dense(100, activation="relu"), 
	keras.layers.Dense(10, activation="softmax") 
])*

## Normalización 
### Ejemplo: 
```python
# Crear la capa de normalización
norm_layer = tf.keras.layers.Normalization(input_shape=X_train.shape[1:])

# Ajustar la capa con las estadísticas de X_train
norm_layer.adapt(X_train)
```
Aquí se defina la capa normalizada por medio de *tf.keras.layers.Normalization*, a esta función hay que definirle el forma en la que entran los datos (Cuantas características tiene), para tomar la forma de los datos comúnmente se toma la forma (.shape) del conjunto de datos, pero a partir de la posición 1, ya que la posición 0 son el numero de filas (instancias).

Además cuando se hace la normalización es necesario añadir el método  **`adapt()`**  para que capture las estadísticas necesarias para la normalización (Media y desviación estándar).
# Definición de conjuntos de datos

Es necesario tener conjuntos de **entrenamiento**, **validación** y **prueba** para garantizar que el modelo aprenda correctamente y su rendimiento sea evaluado de manera adecuada. Cada conjunto cumple un propósito diferente en el desarrollo y evaluación de un modelo de Machine Learning.

| **Conjunto**      | **Propósito**                                                                               | **Uso en el modelo**                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Entrenamiento** | Ajustar los parámetros del modelo minimizando la pérdida.                                   | El modelo aprende patrones generales en este conjunto.                                                              |
| **Validación**    | Evaluar el rendimiento mientras se entrena y ajustar hiperparámetros (no afecta los pesos). | Se evalúa durante el entrenamiento para decidir cuándo detenerse (*early stopping*) o cómo ajustar hiperparámetros. |
| **Prueba**        | Evaluar el rendimiento final del modelo en datos completamente nuevos (nunca usados antes). | Se usa únicamente al final para medir la capacidad de generalización a datos del mundo real.                        |
# Compilación 

En la compilación indicamos la configura del modelo para el entrenamiento. Esto implica definir los elementos necesarios para que el modelo pueda aprender y ajustarse a los datos. Tiene los siguientes elementos:

## Función de perdida 

La métrica que el modelo usará para medir el error entre las predicciones y las etiquetas reales.

Ejemplo: 
-  *Categorical Crossentropy:* cuando tus etiquetas están en formato **one-hot encoding**. Cada etiqueta es un vector binario donde solo una posición tiene un valor 1 (indicando la clase verdadera) y las demás son 0. ejemplo: Clase 0: [1, 0, 0]
- *Sparse Categorical Crossentropy:* cuando tus etiquetas son **índices enteros** (más común porque requiere menos preprocesamiento). Las etiquetas son **índices enteros** que representan directamente la clase. ejemplo: Clase 0: 0

## Optimizador

El optimizador es el algoritmo que ajusta los pesos del modelo durante el entrenamiento para minimizar la pérdida.

 - *SGD (Stochastic Gradient Descent):* Método básico para el descenso de gradiente

## Métricas 

Las métricas se usan para monitorear el rendimiento del modelo. Aunque no afectan el entrenamiento, son útiles para evaluar cómo está funcionando el modelo en cada época.

- *accuracy:* Porcentaje de predicciones correctas (en clasificación).
- *Mae:* Error absoluto promedio (en regresión).

# Modelos

## Regresión

```python
# Crear la capa de normalización
norm_layer = tf.keras.layers.Normalization(input_shape=X_train.shape[1:])

# Ajustar la capa con las estadísticas de X_train
norm_layer.adapt(X_train)

# Construir el modelo con la capa ya ajustada
model = tf.keras.Sequential([
    norm_layer,
    tf.keras.layers.Dense(50, activation="relu"),
    tf.keras.layers.Dense(50, activation="relu"),
    tf.keras.layers.Dense(50, activation="relu"),
    tf.keras.layers.Dense(1)
])

# Compilar el modelo
optimizer = tf.keras.optimizers.Adam(learning_rate=1e-3)
model.compile(loss="mse", optimizer=optimizer, metrics=["RootMeanSquaredError"])

# Entrenar el modelo
history = model.fit(X_train, y_train, epochs=20, validation_data=(X_valid, y_valid))
```

*Nota:* El método **`adapt()`** es necesario porque la capa de normalización (`Normalization`) de Keras necesita calcular las **estadísticas** del conjunto de datos (como la **media** y la **desviación estándar**) antes de poder normalizar correctamente las entradas. 

## Tipos de redes

Hasta el momento hemos visto como se estructuran las redes secuenciales, pero hay otro tipo de redes.

# Keras Tuner

Keras Tuner es una herramienta que **busca automáticamente los mejores hiperparámetros** para entrenar un modelo de Keras.

- En lugar de probar manualmente diferentes valores de hiperparámetros, Keras Tuner **ajusta automáticamente** el modelo usando métodos como **Grid Search, Random Search o Bayesian Optimization**.

### 🔹 **Ejemplos de hiperparámetros a ajustar:**

- Número de capas ocultas (`n_hidden`).
- Número de neuronas en cada capa (`n_neurons`).
- Tasa de aprendizaje (`learning_rate`).
- Tipo de optimizador (`optimizer`).