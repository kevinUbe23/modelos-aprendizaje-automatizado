## Librerías principales

- **pandas**: 2.1.x  
- **numpy**: 1.26.x  
- **scikit-learn**: 1.4.x  
  - `StandardScaler`, `KMeans`, `DBSCAN`, `PCA`, `TSNE`, `silhouette_score`
- **matplotlib**: 3.8.x  
- **seaborn**: 0.13.x  

Instalación sugerida (entorno local):

```bash
pip install pandas numpy scikit-learn matplotlib seaborn

# Segmentación de clientes con aprendizaje no supervisado

Este proyecto implementa y analiza modelos de **aprendizaje no supervisado** (K-means, DBSCAN, PCA y t-SNE) para segmentar clientes de una plataforma de comercio electrónico a partir de su comportamiento de compra.

---

## 1. Dataset

Se utiliza un dataset de **350 clientes** con las siguientes variables principales:

- Perfil: `Gender`, `Age`, `City`, `Membership Type`.
- Comportamiento: `Total Spend`, `Items Purchased`, `Average Rating`, `Discount Applied`, `Days Since Last Purchase`.
- `Customer ID` y `Satisfaction Level` se usan solo para identificación y análisis, no entran al modelo.

Este conjunto combina atributos demográficos y de comportamiento, lo que lo hace adecuado para construir perfiles de clientes.

---

## 2. Metodología

### 2.1. Análisis exploratorio (EDA)

- Descripción general de variables numéricas (rangos, medias, outliers) y categóricas (distribuciones por género, ciudad, membresía y descuentos).
- Matriz de correlación para identificar relaciones entre:
  - `Total Spend` e `Items Purchased`.
  - `Days Since Last Purchase` y variables de gasto/frecuencia.

### 2.2. Preprocesamiento

1. Eliminación de columnas:
   - `Customer ID` (identificador).
   - `Satisfaction Level` (se conserva aparte para interpretación de clusters).
2. Codificación de categóricas con **One-Hot Encoding**:
   - `Gender`, `City`, `Membership Type`, `Discount Applied`.
3. Construcción de la matriz de características:
   - `df_encoded` contiene todas las variables numéricas y dummies.
   - `X = df_encoded.values`.
4. Estandarización con `StandardScaler`:
   - `X_scaled` se utiliza en todos los modelos no supervisados.

---

## 3. Modelos de clustering

### 3.1. K-means

- Se evaluaron k = 2…10 con:
  - **Método del codo** (inercia/WCSS).
  - **Índice de silueta**.
- El codo aparece alrededor de k ≈ 5–6 y el silhouette alcanza valores máximos en k ≈ 6–7.
- Se seleccionó **k = 6** como compromiso entre:
  - Buena separación de clusters.
  - Complejidad e interpretabilidad razonables.
- Se entrena `KMeans(n_clusters=6, random_state=42)` sobre `X_scaled` y se añaden las etiquetas al DataFrame.

### 3.2. DBSCAN

- Búsqueda de parámetros sobre una rejilla de `eps` y `min_samples`.
- Se selecciona una configuración que produce ~7 clusters con silhouette alto y cierta cantidad de ruido, lo que permite detectar:
  - Grupos densos de clientes.
  - Puntos atípicos que K-means obliga a asignar a algún cluster.

---

## 4. Reducción de dimensionalidad

### 4.1. PCA (2 componentes)

- Se aplica `PCA(n_components=2)` sobre `X_scaled`.
- Las dos primeras componentes concentran la mayor parte de la varianza relevante.
- Gráficos PCA 2D coloreados por:
  - Clusters de K-means.
  - Clusters de DBSCAN.
- Los grupos aparecen claramente separados, lo que respalda la estructura detectada por los algoritmos de clustering.

### 4.2. t-SNE (2D)

- Se aplica `TSNE(n_components=2, perplexity=30, random_state=42)` sobre `X_scaled`.
- El mapa t-SNE muestra grupos muy compactos y revela microestructuras que PCA (lineal) no capta tan claramente.
- Se generan gráficos t-SNE coloreados por K-means y DBSCAN para comparar las particiones.

---

## 5. Interpretación de clusters

Para interpretar los segmentos se construye un DataFrame:

- `df_km_vars = df_encoded.copy()`
- Se añade la columna `KMeans_Cluster`.

Se analizan:

- **Medias por cluster** de todas las variables del modelo (`df_km_vars.groupby("KMeans_Cluster").mean()`), lo que permite identificar:
  - Clusters de **bajo valor** (bajo `Total Spend`, pocos `Items Purchased`, `Days Since Last Purchase` alto).
  - Clusters de **valor medio** (variables en rangos intermedios, comportamiento estable).
  - Clusters de **alto valor recurrente** (alto gasto, alta frecuencia, baja recencia, mayor proporción de membresías Gold/Silver y uso de descuentos).
  - Clusters dominados por ciertas **ciudades o género**, observando las medias de las dummies (`City_*`, `Gender_Male`, etc.).
- Se muestran ejemplos de registros (`head()`) por cluster para inspeccionar casos concretos.

---

## 6. Comparación de métodos

- **K-means**
  - Produce una partición completa (todos los clientes pertenecen a un cluster).
  - Es sencillo de implementar y de usar para etiquetar nuevos clientes.
  - Adecuado para segmentación operativa (dashboards, campañas, reglas de negocio).

- **DBSCAN**
  - No requiere fijar k; descubre **clusters por densidad** y detecta **ruido**.
  - Es útil para resaltar grupos densos y clientes atípicos que podrían ser anómalos o casos especiales.
  - Más sensible a la elección de `eps` y `min_samples`.

- **PCA vs t-SNE**
  - PCA confirma la estructura global de los clusters en un espacio lineal reducido.
  - t-SNE resalta mejor grupos compactos y separaciones no lineales.

---

## 7. Limitaciones y trabajo futuro

**Limitaciones**

- Dataset de tamaño moderado y con número limitado de variables (no incluye canal de adquisición, tipo de producto, dispositivo, etc.).
- K-means asume clusters aproximadamente esféricos; DBSCAN es muy sensible a los parámetros.
- No se ha enlazado aún cada segmento con KPIs de negocio (ingresos, margen, respuesta a campañas).

**Trabajo futuro**

- Incluir nuevas variables de comportamiento y contexto.
- Probar otros algoritmos de clustering (mezclas gaussianas, clustering jerárquico).
- Evaluar la estabilidad de los clusters en el tiempo.
- Validar los segmentos con el área de marketing y medir su impacto en campañas reales.

---
