# Clasificación técnica con SVM y Árboles de Decisión

## Resumen
Actividad para comparar y evaluar técnicas de clasificación supervisada aplicadas a un problema técnico. Se implementan y analizan modelos Support Vector Machines (SVM) y Árboles de Decisión, desde la preparación de datos hasta la validación y evaluación de resultados.

## Objetivos
- Implementar pipelines reproducibles para SVM y Árboles de Decisión.
- Preprocesar y seleccionar características relevantes.
- Comparar rendimiento con métricas estándar (accuracy, precision, recall, F1, ROC AUC).
- Documentar conclusiones y mejores prácticas.

## Datos
- Incluir una breve descripción del dataset usado (columnas, tipo de variables, tamaño).
- Indicar fuente o archivo local (p. ej. data/dataset.csv).
- Recomendación: dividir en train/validation/test o usar validación cruzada.
- Utilizamos el Dataset: 
  Nombre: Server Logs - Suspicious
  Enlace: https://www.kaggle.com/datasets/kartikjaspal/server-logs-suspicious

## Metodología
1. Inspección y limpieza de datos (valores faltantes, duplicados, tipos).
2. Ingeniería de características y escalado (escalado estándar para SVM).
3. Selección de características (filtros, wrappers o embedded).
4. Entrenamiento de modelos:
    - Árbol de Decisión (tuning: max_depth, min_samples_split, criterio).
    - SVM (kernel lineal/RBF, C, gamma).
5. Validación y ajuste de hiperparámetros (GridSearchCV/RandomizedSearchCV).
6. Evaluación final sobre conjunto de test y análisis de errores.

## Métricas de evaluación
- Accuracy
- Precision, Recall, F1-score (por clase y macro/micro)
- Matriz de confusión
- ROC AUC (si corresponde)
- Curvas de aprendizaje y validación

## Requisitos
- Python 3.8+
- Paquetes sugeridos: scikit-learn, pandas, numpy, matplotlib, seaborn, joblib
- Instalar: pip install -r requirements.txt

## Estructura del repositorio (sugerida)
- data/                     -> datasets (raw y procesados)
- notebooks/                -> notebooks de exploración y experimentos
- src/                      -> scripts y módulos reutilizables
- results/                  -> modelos entrenados y métricas
- README.md
- requirements.txt

## Uso rápido
- Preparar entorno: pip install -r requirements.txt
- Ejecutar notebook principal: notebooks/experimentos.ipynb
- Scripts de entrenamiento: python src/train.py --model svm --config configs/svm.yaml

## Buenas prácticas
- Guardar pipelines y modelos (joblib/pickle) con versión de librerías.
- Registrar experimentos y parámetros (ej. MLflow, CSV).
- Realizar análisis de importancia de características y sensibilidad.

## Resultados y conclusiones
- Resumen numérico de rendimiento por modelo.
- Comparación interpretativa: ventajas/desventajas de SVM vs Árbol de Decisión en el contexto técnico del dataset.
- Recomendaciones para producción y pasos siguientes.

## Licencia y autores
- Licencia: MIT (o la que corresponda)
- Autor: Nombre del equipo o persona responsable
- Fecha: YYYY-MM-DD

Notas: adaptar secciones a los datos y objetivos específicos de la actividad.
