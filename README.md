# Análisis de Inseguridad por Estados usando K-Means

[**➔ Ver en Google Colab**](https://colab.research.google.com/github/AndrewP05/Kmeans_estados_inseguridad/blob/main/kmeans_estados_inseguridad.ipynb)

**Objetivo:** El propósito de este cuaderno es agrupar (clusterizar) estados según sus índices de criminalidad y características demográficas. 

Para lograrlo, utilizaremos el algoritmo de aprendizaje no supervisado **K-Means**, analizando las siguientes variables:
* `Murder`: Tasa de asesinatos.
* `Assault`: Tasa de asaltos.
* `Rape`: Tasa de violaciones.
* `UrbanPop`: Porcentaje de población urbana.

## 1. Configuración del Entorno
Dado que estamos trabajando en Google Colab, el primer paso es montar nuestro Google Drive para poder acceder a los directorios y archivos de datos necesarios para el análisis.

## 2. Importación de Librerías
Importamos las herramientas fundamentales para nuestro análisis:
* **Manipulación de datos:** `pandas`, `numpy`.
* **Visualización:** `matplotlib`, `seaborn`, `Axes3D` (para gráficos en 3D).
* **Machine Learning (Scikit-Learn):** `KMeans` (para el algoritmo de agrupamiento), `MaxAbsScaler` (para normalizar la escala de los datos) y `silhouette_score` (para evaluar la calidad y cohesión de los clusters formados).

## 3. Carga y Exploración Inicial de los Datos
Cargamos el dataset `datos_estados.csv` que contiene las métricas de inseguridad por cada estado. Utilizamos la función `head()` para visualizar las primeras filas y comprobar que las variables numéricas y los nombres de los estados se han cargado correctamente.

| Estado | Murder | Assault | UrbanPop | Rape |
| :--- | ---: | ---: | ---: | ---: |
| Alabama | 13.2 | 236 | 58 | 21.2 |
| Alaska | 10.0 | 263 | 48 | 44.5 |
| Arizona | 8.1 | 294 | 80 | 31.0 |
| Arkansas | 8.8 | 190 | 50 | 19.5 |
| California | 9.0 | 276 | 91 | 40.6 |

## 4. Preprocesamiento: Escalado de los Datos
Los algoritmos basados en cálculo de distancias (como K-Means) son muy sensibles a las diferencias de proporciones numéricas. Si una variable tiene valores absolutos mucho más altos que otras (por ejemplo, `Assault` suele tener valores de cientos, frente a `Murder` que tiene decenas), la variable mayor dominará la asignación de los clusters. 

Para evitar este sesgo, aplicamos `MaxAbsScaler`. Esta técnica escala cada característica dividiéndola por su valor máximo absoluto, dejando todas las variables numéricas en un rango equilibrado (hasta 1.0). Finalmente, reconstruimos el DataFrame manteniendo la columna categórica `Estado`.

| Murder | Assault | UrbanPop | Rape | Estado |
| ---: | ---: | ---: | ---: | :--- |
| 0.758621 | 0.700297 | 0.637363 | 0.460870 | Alabama |
| 0.574713 | 0.780415 | 0.527473 | 0.967391 | Alaska |
| 0.465517 | 0.872404 | 0.879121 | 0.673913 | Arizona |
| 0.505747 | 0.563798 | 0.549451 | 0.423913 | Arkansas |
| 0.517241 | 0.818991 | 1.000000 | 0.882609 | California |

## 5. Construcción del Modelo K-Means y Visualización
En esta fase determinamos la cantidad adecuada de clusters (evaluando métricas como el coeficiente de silueta) y procedemos a ajustar el modelo K-Means con nuestros datos escalados. Posteriormente, representaremos gráficamente la separación de los estados.


## 6. Interpretación de los Clusters
Una vez que el modelo ha asignado cada estado a un grupo (columna `Cluster`), procedemos a interpretar qué representa cada uno. 
En esta sección imprimimos los estados que conforman cada cluster y extraemos las **estadísticas promedio** (`Murder`, `Assault`, `UrbanPop`, `Rape`). Esto es vital para asignarle un "perfil" a cada grupo (por ejemplo: "Estados de altísima criminalidad" vs. "Estados seguros con baja población urbana").

## 7. Conclusión y Hallazgos Principales 

Tras aplicar el algoritmo de **K-Means** a nuestro conjunto de datos de criminalidad por estados, hemos logrado segmentar el país en grupos distintos con características muy marcadas. El uso de la escala máxima absoluta (`MaxAbsScaler`) fue fundamental para garantizar que delitos con altas tasas, como los asaltos, no opacaran a otros crímenes graves pero numéricamente menores, como los asesinatos.

A partir de las estadísticas promedio de cada cluster, podemos concluir lo siguiente:

1. **Relación entre Población Urbana y Delito:** El análisis revela una tendencia interesante entre la variable `UrbanPop` y las tasas de criminalidad. Los clusters con mayor proporción de población urbana tienden a presentar tasas notablemente más altas en delitos como asaltos (`Assault`) y violaciones (`Rape`), sugiriendo que las áreas metropolitanas enfrentan desafíos de seguridad diferentes a las zonas rurales.
   
2. **Identificación de Zonas Críticas:** El modelo logró agrupar exitosamente a los estados con los índices más preocupantes de criminalidad (como lo demuestra el cluster con promedios altos de `Murder` y `Assault`). Estos son los estados que requerirían mayor atención en políticas públicas de seguridad.

3. **Zonas de Mayor Seguridad:** En contraste, el algoritmo también agrupó aquellos estados que destacan por sus bajas tasas en todos los delitos, sirviendo como posibles modelos de estudio para entender qué políticas preventivas están funcionando.

**Reflexión Final:**
El aprendizaje no supervisado (K-Means) demostró ser una herramienta muy eficaz para descubrir patrones subyacentes en datos sociodemográficos y de seguridad. Esta clusterización proporciona una base sólida para que los tomadores de decisiones puedan asignar recursos de seguridad y estrategias de prevención de forma más focalizada, entendiendo que el perfil criminal varía drásticamente de un grupo de estados a otro.
