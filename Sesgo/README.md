# Detección y Explicación de Tráfico de Red Sospechoso

Proyecto de **clasificación de tráfico de red** en dos clases:  
- `0` → tráfico **normal**  
- `1` → tráfico **sospechoso / ataque**  

El trabajo combina **modelos de Machine Learning** (Random Forest) con técnicas de **explicabilidad (XAI)**: SHAP y LIME, para entender qué características del tráfico llevan al modelo a catalogar un flujo como anómalo.

---

## 1. Descripción del problema

Se dispone de un dataset de flujos de red con información típica de conexiones, entre otras:  

- `timestamp`  
- `duration`  
- `Src IP Addr`, `Dst IP Addr`  
- `Src Pt`, `Dst Pt` (puertos de origen y destino)  
- `Packets`, `Bytes`  
- `Proto` (protocolo)  
- `Flags`, `Tos`  
- `class` (Normal / ataque)  
- `attackType`, `attackID`, `attackDescription`  

El objetivo es **detectar automáticamente tráfico sospechoso** y, además, **explicar** por qué el modelo toma cada decisión.

---

## 2. Objetivos

1. Preparar y limpiar el dataset de tráfico de red.  
2. Entrenar un modelo de clasificación supervisada (Random Forest).  
3. Evaluar su rendimiento con métricas estándar y matriz de confusión.  
4. Aplicar técnicas de explicabilidad global (SHAP) y local (LIME).  
5. Analizar qué variables son más relevantes para detectar anomalías.  

---

## 3. Metodología

### 3.1. Preprocesamiento

- **Definición del target**  
  - `Y = 0` → Normal  
  - `Y = 1` → Sospechoso / Ataque

- **Selección de variables**  
  - Se eliminan columnas que pueden inducir al modelo a “memorizar identidades” en lugar de aprender patrones (por ejemplo, IPs o descripciones textuales del ataque).

- **Tratamiento de tipos**  
  - Conversión de columnas numéricas (`duration`, `bytes`, `packets`, etc.) a formato numérico.  
  - Conversión de columnas categóricas (`Proto`, `Flags`, etc.) a texto y posterior codificación con `LabelEncoder`.

- **Partición de datos**  
  - `train_test_split` estratificado para mantener la proporción de normales/anómalos.  

### 3.2. Modelado

- **Modelo**: `RandomForestClassifier` con `max_depth=8` para evitar sobreajuste extremo y facilitar la explicación de las reglas aprendidas.
- **Evaluación**:  
  - `accuracy`, `precision`, `recall`, `f1-score`.  
  - Matriz de confusión visual (heatmap), que muestra un rendimiento muy alto con pocos falsos positivos/negativos.

### 3.3. Explicabilidad (XAI)

1. **SHAP (global)**
   - Se utiliza `TreeExplainer` sobre el Random Forest.  
   - Se generan:
     - **Beeswarm plot** de valores SHAP para ver la influencia de cada variable en todas las predicciones.  
     - **Gráfico de barras** con la importancia media absoluta de las variables.  
   - Las variables más influyentes para detectar anomalías incluyen:
     - `Src Pt`, `Dst Pt`  
     - `Duration`  
     - `Bytes`, `Packets`  
     - `Flags`, `Proto`  

2. **LIME (local)**
   - Se configura `LimeTabularExplainer` con las características de entrenamiento.  
   - Se selecciona una instancia clasificada como **anomalía** y se explica la predicción.  
   - El gráfico LIME muestra qué condiciones (por ejemplo, duración alta, ciertos rangos de puertos o de bytes) empujan la probabilidad hacia “Anomalía”.  

---

## 4. Resultados principales

- El modelo Random Forest presenta una **tasa de acierto muy alta**, con una matriz de confusión casi perfecta en el conjunto de prueba.
- SHAP revela que los **puertos, la duración y el volumen de tráfico** son determinantes en la detección de ataques.
- LIME permite **explicar casos individuales**, útil para analistas de seguridad que requieren justificar por qué un flujo fue marcado como sospechoso.

---