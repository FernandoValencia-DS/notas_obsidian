---
{"dg-publish":true,"permalink":"/flujo-de-trabajo-machine-learning/","dgPassFrontmatter":true}
---


Una guía práctica para preparar correctamente los datos antes de entrenar modelos de clasificación (con especial foco en redes neuronales, SVM y modelos sensibles al escalado).

---

## 📌 1. División del dataset

**Objetivo:** Separar el dataset en conjuntos que no compartan información.

```python
from sklearn.model_selection import train_test_split

# División en entrenamiento (60%), validación (20%) y prueba (20%)
X_train, X_temp, y_train, y_temp = train_test_split(X, y, test_size=0.4, random_state=42)
X_val, X_test, y_val, y_test = train_test_split(X_temp, y_temp, test_size=0.5, random_state=42)
```

---

## 📌 2. Preprocesamiento según tipo de variable

### 🔷 Variables continuas
- Escalado recomendado:
  - `StandardScaler` si los datos tienen distribución normal.
  - `MinMaxScaler` si tienen un rango definido.
  - `RobustScaler` si hay valores atípicos.

### 🔷 Variables ordinales
- Pueden escalarse como si fueran continuas.
- Escalado sugerido: `MinMaxScaler` o `StandardScaler`.

### 🔷 Variables categóricas
- Convertir a **One-Hot Encoding** si no tienen orden lógico.

```python
import pandas as pd
data = pd.get_dummies(data, columns=['MARRIAGE'], drop_first=True)
```

- Para variables binarias (ej. SEX: 1 = masculino, 2 = femenino), se puede binarizar:

```python
data['SEX'] = data['SEX'].map({1: 0, 2: 1})
```

---

## 📌 3. Escalado de datos

⚠️ **Solo se debe ajustar el escalador con el conjunto de entrenamiento** para evitar **fugas de datos**.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_val_scaled = scaler.transform(X_val)
X_test_scaled = scaler.transform(X_test)
```

---

## 📌 4. Opcional: Preprocesamiento con `ColumnTransformer`

Permite aplicar transformaciones específicas por tipo de variable:

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler

preprocessor = ColumnTransformer(transformers=[
    ('num', StandardScaler(), ['AGE', 'LIMIT_BAL']),
    ('cat', 'passthrough', ['SEX', 'MARRIAGE_1', 'MARRIAGE_2'])
])

X_train_scaled = preprocessor.fit_transform(X_train)
X_val_scaled = preprocessor.transform(X_val)
X_test_scaled = preprocessor.transform(X_test)
```

---

## 📌 5. Balanceo de clases (si es necesario)

Cuando las clases están desbalanceadas (por ejemplo, muchos ejemplos de clase 0 y pocos de clase 1), el modelo tiende a favorecer la clase mayoritaria. Aquí tienes las técnicas más utilizadas para corregir esto:
### Opción 1: SMOTE (Sobremuestreo sintético)

**SMOTE** (Synthetic Minority Over-sampling Technique) crea ejemplos sintéticos para la clase minoritaria a partir de combinaciones lineales de ejemplos reales cercanos.

```python
from imblearn.over_sampling import SMOTE
smote = SMOTE(sampling_strategy=0.8, random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)
```
- ✅ Aumenta la clase minoritaria sin duplicar ejemplos.
    
- ⚠️ Puede generar puntos irreales si la distribución original es muy dispersa.
    
- Ideal para modelos sensibles al balance como MLP o SVM.
### Opción 2: Submuestreo aleatorio

Elimina ejemplos aleatorios de la clase mayoritaria para igualar la cantidad de ejemplos con la clase minoritaria.

```python
from imblearn.under_sampling import RandomUnderSampler
rus = RandomUnderSampler(random_state=42)
X_resampled, y_resampled = rus.fit_resample(X_train, y_train)
```
- ✅ Simple y rápido.
    
- ⚠️ Puede eliminar información importante de la clase mayoritaria.
    
- Útil si tienes muchos datos o poca memoria.

### Opción 3: SMOTE + Tomek (técnica híbrida)

**SMOTETomek** combina SMOTE (para generar ejemplos de la clase minoritaria) y **Tomek Links** (para eliminar ejemplos ruidosos de la clase mayoritaria que están demasiado cerca de los de la clase minoritaria).
 ```python
 from imblearn.combine import SMOTETomek
smote_tomek = SMOTETomek(sampling_strategy=0.8, random_state=42)
X_resampled, y_resampled = smote_tomek.fit_resample(X_train, y_train)
```
- ✅ Mejora el balance y también **limpia el límite entre clases**.
    
- ⚠️ Puede eliminar ejemplos valiosos si el límite entre clases es difuso.
    
- Ideal para modelos que se benefician de una buena separación de clases (como redes neuronales o SVM).

### 🤨 ¿Cuál elegir?

| Técnica       | Pros                                     | Contras                             | Ideal para...           |
| ------------- | ---------------------------------------- | ----------------------------------- | ----------------------- |
| SMOTE         | Crea ejemplos sin eliminar datos         | Puede crear ruido                   | MLP, SVM                |
| Undersampling | Simple, reduce tiempo de entrenamiento   | Pierde información                  | Árboles, Random Forest  |
| SMOTE + Tomek | Balancea y limpia el límite entre clases | Puede ser agresivo eliminando datos | Modelos complejos (MLP) |

---
## ✅ Buenas prácticas finales

- 🔒 **Evita fuga de datos**: nunca uses info del test o validación para escalar o balancear.


