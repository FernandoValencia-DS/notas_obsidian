---
{"dg-publish":true,"permalink":"/redes-neuronales/"}
---

Las redes neuronales son una rama de la [[Inteligencia Artificial\|Inteligencia Artificial]]
# TLU (Threshold Logic Unit)
Un **TLU** es una unidad de procesamiento de datos en la que se calculan entradas ponderadas (una combinación lineal de los datos de entrada), y luego se compara con un **umbral**. Si el resultado de la combinación supera ese umbral, la unidad devuelve una salida positiva (por ejemplo, 1); si no, devuelve una salida negativa (por ejemplo, 0). Es una forma muy básica de clasificador binario. Este comportamiento es similar al de un clasificador **logístico** o **SVM lineal**, que también buscan separar dos clases linealmente.

## Sesgo
**Bias (sesgo)**: El **sesgo** es un parámetro adicional en un modelo de Machine Learning que permite al modelo hacer predicciones más flexibles. Es como un "ajuste" que permite que la función de activación (por ejemplo, la función escalón del TLU o la sigmoide de una red neuronal) se desplace hacia arriba o hacia abajo en el eje vertical.

En un modelo como un TLU o un perceptrón, el **sesgo** se introduce como una característica adicional $x_0​$ que siempre tiene el valor de **1**. Esto asegura que el modelo tenga la capacidad de aprender un valor de sesgo $w_0$ ​,que se ajusta durante el entrenamiento.
Si no añades un **sesgo** (es decir, si no incluyes $x_0 = 1$), el modelo estaría forzado a pasar por el origen (0, 0) en un gráfico bidimensional (o el origen en dimensiones más altas). Esto puede ser limitante, ya que en muchos problemas de clasificación, la mejor separación de clases no pasa por el origen.

## Dense Layer

Es cuando todas las neuronas de una capa (_layer_) están conectadas con cada neurona de la capa anterior.

## backpropagation

Es fundamental para entrenar redes neuronales y se basa en la idea de minimizar el error que la red comete al hacer predicciones.

1. **Predicción y Cálculo del Error (Forward Pass)**:
    
    - Primero, se pasa la entrada a través de la red neuronal (esto es el _forward pass_). Cada neurona toma los valores de entrada, los multiplica por los pesos de sus conexiones, les aplica una función de activación y pasa el resultado a las neuronas de la siguiente capa.
    - Al final de este proceso, obtienes la salida de la red, que es la predicción.
    - Después, calculas el error comparando esta predicción con la salida esperada (real), generalmente usando una función de pérdida, como el error cuadrático medio.
2. **Propagación del Error Hacia Atrás (Backward Pass)**:
    
    - Aquí comienza la parte más compleja, que es la propagación del error hacia atrás a través de la red. El objetivo es calcular cuánto contribuyó cada conexión al error final.
        
    - El cálculo de este "error" en cada conexión se hace utilizando la regla de la cadena de la derivada (la derivada de la función de activación y la derivada de la función de pérdida con respecto a las salidas).
        
    - Para entender esto con más detalle:
        
        - El error en la salida (última capa) se calcula primero. Este error es la diferencia entre la salida real y la salida predicha.
        - Luego, este error se "propaga" hacia atrás, capa por capa. Para hacer esto, calculamos cómo cambia el error en función de los pesos de cada capa, es decir, calculamos la derivada del error con respecto a cada peso.
        - Usamos la **regla de la cadena** para distribuir el error a través de las conexiones y cada neurona. Este paso se repite desde la capa de salida hasta la capa de entrada.
3.  **Ajuste de los Pesos (Gradient Descent)**:
    
    - Después de calcular cómo cada peso contribuyó al error, ajustamos los pesos en la dirección que reduce el error. Esto se hace usando un algoritmo llamado _gradiente descendente_.
    - El gradiente descendente utiliza la derivada del error con respecto a cada peso para hacer un pequeño ajuste en la dirección opuesta al gradiente, es decir, para disminuir el error.
    - Este ajuste de los pesos se repite en cada iteración del proceso de entrenamiento.

**Resumen**: El algoritmo de _backpropagation_ mide el error total al final de la red (durante el _forward pass_), y luego, durante el _backward pass_, distribuye este error hacia atrás a través de la red para determinar cuánto contribuye cada conexión (y cada peso) al error global. Esto se logra calculando las derivadas del error con respecto a cada peso y luego ajustando esos pesos en el paso de _gradient descent_ para reducir el error en la siguiente iteración.

**Importante**: Es necesario iniciar todos los pesos de manera aleatoria, ya que si se hace por ejemplo con todas en 0, al hacer el _backpropagation_ este ajustará por igual todas las neuronas, haciendo que sea igual a tener una neurona por capa.

## Resumen regresión y Clasificación con MLP
![{1577D8CB-66CC-4D10-AFFF-1C8B8D2F7294}.png](/img/user/Imagenes/%7B1577D8CB-66CC-4D10-AFFF-1C8B8D2F7294%7D.png)
![{68770D74-CFD8-466E-84AC-2A79F093CB76}.png](/img/user/Imagenes/%7B68770D74-CFD8-466E-84AC-2A79F093CB76%7D.png)
# Tipos de redes

Hasta el momento hemos hablado de las redes secuenciales, en las que las capas de neuronas están conectadas de manera consecutiva. Pero hay otros tipos.
## Deep & Wide

Como lo indica su nombre es tiene una parte profunda denominada _Deep_ ya que pasa por las neuronas y otra que pasa directamente desde la entrada a la salida y es denominada _wide_. Estas pueden tener una única entrada o múltiples entradas que permitan separa las características que pasan por cada fase.
![{02A20F62-4E4E-4736-8D5D-9F0DB5C48FE3}.png](/img/user/Imagenes/%7B02A20F62-4E4E-4736-8D5D-9F0DB5C48FE3%7D.png)![{526D2C56-A777-40BD-9173-3C37B017E5F3}.png](/img/user/Imagenes/%7B526D2C56-A777-40BD-9173-3C37B017E5F3%7D.png)
