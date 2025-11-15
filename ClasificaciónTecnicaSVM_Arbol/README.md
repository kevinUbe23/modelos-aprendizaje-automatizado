### Clasificación técnica mediante SVM, Árboles de Decisión y Random Forest aplicados a la detección de conexiones sospechosas
---

### Métricas de evaluación
- Accuracy
- Precision, Recall, F1-score (por clase y macro/micro)
- Matriz de confusión
- ROC AUC (si corresponde)
- Curvas de aprendizaje y validación

### Requisitos
- Python 3.8+
- Instalar: pip install -r requirements.txt

### Estructura del repositorio
- content/ -> dataset (raw)
- graphs/ -> gráficos de conclusión
- ClasificaciónTecnicaSVM_ArbolDecision_Grupo5.ipynb -> scripts y módulos
- README.md
- requirements.txt

### Resumen del problema

El estudio tiene como objetivo desarrollar, entrenar y comparar clasificadores supervisados (específicamente **Máquinas de Vectores de Soporte (SVM)** y **Árboles de Decisión**) aplicados al reconocimiento de patrones en datos reales o simulados, como sensores, comportamiento de usuarios, métricas de rendimiento, entre otros. El propósito central es determinar la eficacia de estos modelos en la detección de patrones anómalos dentro de conjuntos de datos multivariados, donde las clases se agrupan en categorías tales como *normal*, *suspicious* y *unknown*.

Durante la fase exploratoria se evidenció un desequilibrio de clases, siendo la clase *suspicious* la de mayor representación (≈ 62 %), seguida por *normal* (≈ 29 %) y *unknown* (≈ 9 %). El análisis descriptivo reveló variables numéricas relevantes como *duration*, *packets*, *bytes*, y variables categóricas como *flags* o direcciones IP. Estas características se correlacionaron mediante matrices de calor y boxplots, mostrando la existencia de relaciones no lineales y dispersión significativa en algunas dimensiones.

En síntesis, el problema se orienta a determinar qué modelo logra una mejor capacidad de clasificación multiclase en escenarios con desequilibrio, ruido y variables heterogéneas, buscando un equilibrio entre precisión, interpretabilidad y rendimiento computacional.

---

### Metodología

El proceso metodológico se estructuró en cinco fases principales, conforme se observa en el notebook:

1. **Análisis exploratorio de datos (EDA)**
   Se realizó limpieza, normalización de nombres de columnas y verificación de tipos de datos. Se identificaron valores nulos, se estandarizaron variables temporales (*date_first_seen*, *duration_sec*) y se analizaron distribuciones y correlaciones. La inspección gráfica (matrices de correlación, boxplots e histogramas) permitió detectar la dispersión entre clases y variables relevantes.

2. Preprocesamiento
   Se definieron las variables de entrada (*X*) y la variable objetivo (*y*). Los datos fueron divididos en conjuntos de entrenamiento y prueba (80/20), aplicando pipelines diferenciados para variables numéricas (imputación por mediana y escalado *StandardScaler*) y categóricas (imputación por moda y codificación *OneHotEncoder*). Este enfoque garantizó una transformación reproducible de las características antes del entrenamiento.

3. **Entrenamiento de modelos supervisados**
   Se implementaron y evaluaron los siguientes modelos:

   * **Árbol de Decisión**: Modelo base para interpretar jerarquías de decisión y estimar importancia de variables.
   * **SVM lineal (LinearSVC)**: Utilizado por su capacidad de generalización en espacios de alta dimensionalidad.
   * **Bosque Aleatorio (Random Forest)**: Aplicado por su robustez y manejo de no linealidades.
   * **Regresión Logística Multiclase**: Implementada como modelo interpretable y de referencia.

   Cada modelo fue optimizado mediante GridSearchCV con validación cruzada de tres pliegues, buscando maximizar el puntaje *F1-macro*.

4. **Evaluación del desempeño**
   Para cada modelo se calcularon las métricas de precisión, recall y F1-score (macro y ponderadas), además de las matrices de confusión normalizadas. Se realizó una comparación gráfica mediante barras y mapas de calor para analizar consistencia entre modelos.

5. **Comparación y síntesis de resultados**
   Los resultados experimentales se integraron en una tabla resumen de métricas, mostrando un desempeño superior de los modelos basados en árboles y SVM, con valores de *accuracy* cercanos al 99 %. La metodología concluye con la preparación para el análisis comparativo y la discusión de resultados.


### Conclusiones

El proceso comparativo de los modelos supervisados evidencia que todos los clasificadores alcanzaron un rendimiento sobresaliente, con precisiones y valores F1 superiores al 98 %, demostrando la eficacia del preprocesamiento y la calidad de los datos utilizados. Sin embargo, se observan diferencias significativas en términos de robustez, interpretabilidad y generalización.

1. **Árbol de Decisión**
   Mostró un desempeño notable con una *accuracy* cercana al 99.8 %, y un *F1 macro* de 0.9967. Su principal ventaja radica en la interpretabilidad y en la posibilidad de identificar las variables más influyentes (*flags_AP*, *duration_sec*, *bytes*). No obstante, presenta cierta tendencia al sobreajuste debido a su estructura jerárquica, lo que limita su capacidad de generalizar ante nuevas observaciones.

2. **Máquina de Vectores de Soporte (LinearSVC)**
   Logró un *accuracy* del 99.49 % y *F1 macro* de 0.9892, destacando por su capacidad de generalización en espacios multidimensionales y su estabilidad frente al desbalance de clases. Aunque es menos interpretable que los árboles, su ajuste mediante *GridSearchCV* permitió obtener un modelo equilibrado y eficiente computacionalmente.

3. **Random Forest**
   Superó levemente al resto con una *accuracy* del 99.95 % y *F1 macro* de 0.9989, consolidándose como el modelo más robusto y preciso. Su combinación de múltiples árboles reduce el riesgo de sobreajuste y mejora la estabilidad de las predicciones. Además, ofrece información sobre la importancia de las variables, lo que facilita el análisis explicativo sin sacrificar rendimiento.

4. **Regresión Logística Multiclase**
   Alcanzó resultados también altos (*accuracy* del 98.62 %, *F1 macro* de 0.9722), destacando por su simplicidad e interpretabilidad matemática. Aunque su rendimiento fue ligeramente inferior al de los modelos basados en árboles y SVM, sigue siendo una alternativa adecuada cuando se prioriza la comprensión del modelo sobre la complejidad computacional.

En conjunto, los resultados confirman que los modelos basados en ensambles (Random Forest) y vectores de soporte son los más adecuados para contextos con datos complejos, heterogéneos y desbalanceados. Asimismo, el pipeline de preprocesamiento implementado que incluye imputación, escalado y codificación categórica fue determinante para garantizar la homogeneidad de los datos y maximizar el desempeño de los clasificadores.

La comparación final permite concluir que, si bien todos los enfoques alcanzan niveles de precisión aceptables, Random Forest se posiciona como el modelo óptimo para este tipo de problemas, equilibrando de forma efectiva la precisión, la estabilidad y la capacidad de interpretación de resultados.


### Limitaciones

Hoy los modelos funcionan muy bien en las pruebas, pero hay riesgos si los llevamos al “mundo real”. El principal: las clases no están balanceadas; si una aparece mucho más que las otras, el sistema puede “acostumbrarse” a verla y fallar justo con la menos frecuente. También existe posible sobreajuste (aprende demasiado el set de entrenamiento), variables con demasiadas categorías que complican el aprendizaje, y cambios en los datos con el tiempo que podrían bajar el desempeño. Finalmente, las probabilidades que entregan algunos modelos no siempre son confiables, lo que dificulta fijar umbrales claros para alertas.

* Cuando mezclamos datos de una misma “fuente” en entrenamiento y prueba, las métricas pueden verse mejor de lo que realmente son.
* Si el costo de equivocarse no es el mismo para todas las clases (por ejemplo, pasar por alto un “sospechoso”), las métricas generales pueden engañar.
* Lo que hoy rinde excelente en este dataset podría no rendir igual en otro entorno sin ajustes.

---

### Sugerencias — claras y accionables

**1) Validar mejor antes de decidir**
Separar datos por tiempo o por “fuente” (por ejemplo, por equipo/cliente) para evitar fugas entre entrenamiento y prueba. Además de exactitud y F1, mirar curvas de precisión–recuperación y matrices de confusión para entender errores críticos.

**2) Atender el desbalance**
Dos caminos simples: ponderar más los casos raros o re-muestrear (equilibrar las clases). Luego, ajustar el umbral de decisión según el riesgo de cada tipo de error.

**3) Simplificar variables difíciles**
En lugar de convertir en cientos de columnas los campos con muchos valores (p. ej., direcciones o “flags”), agruparlos por familias o rangos (subredes, grupos de señales) o convertirlos a recuentos/frecuencias. Menos ruido, más estabilidad.

### Gracias Totales!!!
