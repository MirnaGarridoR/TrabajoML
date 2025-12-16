<h1 align="center" style="color:#F26522;">
  Análisis Exploratorio de Datos (EDA) aplicado al Sector Hotelero en España
</h1>

<h3 align="center" style="color:#003A46;">
  Proyecto académico — Aprendizaje Automático  
  CUNEF Universidad
</h3>

---

## 1. Integrantes del equipo y ruta al repositirio en GitHub.

| Apellidos | Nombre | Correo electrónico |
|----------|--------|---------------------|
| **Baquero** | Mario | mario.baquero@cunef.edu
| **Garrido** | Mirna | mpatricia.garrido@cunef.edu
| **Sánchez** | Joan | joan.sanchez@cunef.edu

Ruta HTTPS: https://github.com/MirnaGarridoR/TrabajoML.git

---

## 2. Introducción

El turismo es uno de los motores fundamentales de la economía española y un eje estratégico en la competitividad del país. Dentro del sector, las **pernoctaciones hoteleras** constituyen uno de los indicadores más relevantes para medir la demanda turística y planificar decisiones tanto del sector público como del privado.

Como analistas de datos, nuestro cometido es analizar rigurosamente esta información para extraer conclusiones significativas, detectar patrones y preparar el terreno para futuros modelos predictivos basados en aprendizaje automático.

Este proyecto corresponde a la **Primera Entrega de la Evaluación Continua** de la asignatura *Aprendizaje Automático*, centrada en el **Análisis Exploratorio de Datos (EDA)** y en la correcta formulación del problema de negocio.

---

## 3. Problema de Negocio

###  Contexto empresarial


El Ministerio de Industria y Turismo, así como varias Consejerías de Turismo de las Comunidades Autónomas, han manifestado la necesidad de **disponer de un análisis cuantitativo actualizado y riguroso** sobre el comportamiento de la demanda hotelera en España.

España es uno de los países con mayor actividad turística del mundo. Para que el Gobierno pueda diseñar políticas públicas eficientes, taanto de promoción turística como de planificación de infraestructuras, servicios públicos y estrategias de sostenibilidad, es imprescindible explicarr:

- cómo evoluciona la **demanda de pernoctaciones**,  
- qué diferencias existen entre territorios,  
- cómo se comportan los **viajeros residentes vs no residentes**,  
- qué patrones de **estacionalidad** influyen en la presión turística (verano,primavera, otoño e invierno)  
- y qué indicadores deben monitorizarse para anticipar **picos de demanda**.

Los informes previos disponibles presentan dos limitaciones principales:

1. **Se basan en análisis agregados anuales o trimestrales**, insuficientes para comprender la dinámica real mensual.  
2. **No integran un enfoque orientado a aprendizaje automático**, que permita en fases posteriores desarrollar herramientas predictivas y sistemas de alerta temprana.

Por ello, el Gobierno solicita un **Análisis Exploratorio de Datos (EDA)** exhaustivo que siente las bases para un sistema técnico de apoyo a la decisión pública.


##  Problema identificado : 
> **Estimar el número de turistas por provincias del próximo verano 2026 para planificar con antelación los recursos necesarios para su acogida.** 


El equipo de análisis ha sido encargado de:

> **Realizar un estudio detallado del comportamiento mensual del turismo hotelero en España, analizando patrones por comunidad autónoma, residencia, tipo de indicador y periodo, con el fin de extraer conclusiones útiles para la toma de decisiones y preparar el conjunto de datos para modelos predictivos futuros.**

El objetivo empresarial es disponer de un análisis sólido que permita:

- optimizar precios y capacidad,
- planificar campañas promocionales,
- orientar decisiones de inversión regional,
- y preparar modelos predictivos para futuros escenarios.

---

## 4. Origen y descripción del dataset

El dataset **`per.csv`** proviene de la **Encuesta de Ocupación en Establecimientos Hoteleros (EOH)** del **Instituto Nacional de Estadística (INE)**.  
La tabla original fue exportada y transformada a CSV manteniendo la estructura y los campos relevantes.

### Columnas principales del dataset

| **COLUMNA USADA** | **DESCRIPCIÓN** |
|-------------------|-----------------|
| Total_turismo_nacional | Indicador de nivel territorial (Total Nacional o Provincial). |
| Comunidades_ciudades_autonomas | Comunidad Autónoma o ciudad autónoma donde se ubica el establecimiento. |
| Provincias | Provincia específica del destino turístico. |
| Tipo_viajero | Clasificación según viajero o pernoctación. |
| Año | (int) |
| Mes | (int) |
| Temporada | Clasificación estacional (Verano, Otoño, Invierno, Primavera). |
| Turistas_totales | Cantidad de turistas o pernoctaciones (Variable objetivo). |
| Outliers_zscore | Indicador booleano (True/False) si el dato es considerado outlier. |

Este dataset permite estudiar el comportamiento de la demanda turística por región, por residencia y por temporalidad

---

## 5. Justificación del dataset

Elegimos trabajar con este dataset porque:

- refleja un **fenómeno real** y crítico para España,
- contiene estructura **temporal**, **territorial** y **segmentada**, ideal para un EDA profundo,
- es útil desde un punto de vista **empresarial** y estratégico,
- posee la granularidad necesaria para **modelos de regresión y clasificación** en futuras entregas,
- es coherente para formular un problema realista de aprendizaje automático.
- contiene variables de gran interés para la gestión pública.  


Este dataset también permite obtener insights muy relevantes para:

- cadenas hoteleras,
- aerolíneas,
- consultoras,

---

##  6. Planteamiento científico (Aprendizaje Automático)

Con el objetivo de que este trabajo sea escalable hacia un sistema predictivo, el estudio se formula bajo los principios del **Aprendizaje Automático**.


Se formula un problema de aprendizaje supervisado de regresión.
### 6.1 Target
En la fase de modelado se trabaja con un target transformado para estabilizar varianza y facilitar el aprendizaje:
- target: log_ratio (transformación logarítmica del ratio definido en el notebook de feature engineering).

La elección de un target transformado se justifica por:
- presencia de asimetría y heterocedasticidad en variables de demanda,
- mejora de estabilidad en el entrenamiento,
- reducción de sensibilidad a valores extremos reales.

### 6.2 Vector de atributos (X)
Variables explicativas usadas en el modelado final:
- variables temporales (Año, Mes, y/o codificaciones derivadas),
- variables territoriales (Provincias),
- y otras variables derivadas construidas durante feature engineering (ratios, transformaciones y/o agregaciones).

Las variables categóricas se gestionan según el modelo:
- CatBoost: tratamiento nativo de categóricas,
- XGBoost: codificación One-Hot (cuando se compara).


---

##  7. Metodología del EDA (Primera Entrega) y correcciones aplicadas

La primera fase del proyecto se centró en comprender los datos, validar su consistencia y extraer conclusiones accionables.

### 7.1 Análisis preliminar
- revisión de tipos, duplicados y consistencia por columnas,
- verificación de nulos y valores inconsistentes,
- análisis temporal para confirmar periodicidad mensual.
  

### 7.2 Limpieza y preprocesado
Para garantizar la calidad del análisis, se realizaron las siguientes intervenciones críticas en los datos:

- Tratamiento de "Total Nacional": En la columna "Total_nacional", el valor "Total Nacional" representa las filas en las que los datos representan el total de turistas nacionales por mes. En todas aquellas que no cumpla con esta condición, se imputa el valor "No aplica". Además, en aquellas filas donde se utiliza el valor "Total Nacional" en la columna "Total_nacional" se imputa "No aplica" en los valores nulos de esas mismas filas en las columnas "Comunidades_ciudades_autonomas" y "Provincias".

Por otro lado, también se imputa "No aplica" en aquellas filas de la columna "Provincias" cuando la fila representa el total de turistas por comunidades y ciudades autónomas por mes.

- Reasignación de Ceuta y Melilla: En el dataset original, Ceuta y Melilla aparecían en ocasiones solo como "Comunidades" sin asignación provincial clara, o con formatos inconsistentes. Se imputaron como "Provincias" dentro de sus respectivas categorías para permitir una comparativa homogénea con el resto de provincias españolas. Ahora computan como unidades territoriales equivalentes al resto de provincias para el modelo.

- Conversión Temporal: Se transformaron las columnas Año y Mes en un objeto datetime para facilitar la visualización de series temporales y el ordenamiento cronológico correcto en los gráficos.

###  7.3 Detección de errores
- Nulos: Se verificó la existencia de nulos en la columna Provincias derivados de los agregados nacionales (corregido en el paso 7.2).

- Outliers: 
Un outlier (valor atípico) es una observación que se desvía drásticamente del comportamiento general de los datos. En el sector turístico, estos pueden ser de dos tipos:

    - Outliers Reales (Información): Picos extremos de demanda en agosto o caídas a cero durante el confinamiento COVID. Estos NO deben eliminarse porque reflejan la realidad. 
    - Outliers de Error (Ruido): Fallos de registro o imputación incorrecta de datos que distorsionan las estadísticas. Se detectaron valores atípicos mediante el método Z-Score. Se observaron picos extremos que se correspondían con almeria y burgos unicamente y se decidió eliminarlos siguiendo lo establecido por el z score calculado.

Criterio Propio Aplicado: :Para distinguir entre "pico real" y "error", no utilizamos un simple Z-Score (que habría borrado los veranos), sino un sistema de doble validación (Intersección de Z-Scores). 
Un dato solo se considera ruido y se elimina si cumple simultáneamente estas dos condiciones: 
    - Anomalía Histórica (Z_1): El dato es extraño para la historia de esa misma provincia. 
    - Anomalía Contextual (Z_2): El dato es extraño comparado con el resto de su Comunidad Autónoma en ese mismo mes y    año. Este enfoque es el único que tiene sentido estadístico para datos nuestros datos que son estacionales: 
    
Si solo usáramos el criterio 1, eliminaríamos sistemáticamente los meses de agosto de las zonas de playa (que están a más de 3 desviaciones de la media anual), perdiendo información crítica. Al añadir el criterio 2 ("Contexto"), salvamos esos datos: "Es cierto que Baleares tiene un pico altísimo en agosto (alerta 1), pero como toda la comunidad de Baleares también está alta en esa fecha (alerta 2 no se activa), el dato se valida como correcto y se mantiene".Solo eliminamos el dato cuando es excepcionalmente alto para la provincia Y ADEMÁS no cuadra con lo que está pasando en su región en ese momento.

###  7.4 Exploración visual y estadística

- Evolución temporal de viajeros y pernoctaciones  
- Estacionalidad por mes  
- Comparación entre CCAA  
- Diferencias entre residentes vs no residentes  

### 7.5 Conclusiones del EDA y acciones

Resultados principales (resumen):
- estacionalidad marcada y concentraciones mensuales en periodos concretos,
- fuerte heterogeneidad territorial entre provincias y comunidades,
- diferencias sistemáticas en perfiles temporales que requieren políticas adaptadas.

Acciones derivadas:
- necesidad de sistemas predictivos para anticipar picos de demanda,
- recomendación de monitorización mensual y por provincia,
- preparación de dataset para modelado (features temporales y territoriales).

Entre las conclusiones destacadas para el Gobierno:

- Existe alta concentración de demanda en ciertos meses, lo que requiere planificación reforzada.  
- Las comunidades presentan perfiles turísticos muy diferenciados, lo que sugiere políticas adaptadas.  
- Los picos detectados deben ser considerados para reforzar:
  - transporte público,  
  - personal sanitario,  
  - limpieza urbana,  
  - promoción turística,  
  - y capacidad hotelera.

Se recomienda la implementación futura de modelos predictivos para:

- anticipar saturación turística,  
- optimizar campañas públicas,  
- mejorar el seguimiento de indicadores clave.


## 8. Metodología de modelado (entrega final)

### 8.1 Separación del dataset y validación

Dado que los datos tienen estructura temporal, se evita cualquier aleatorización que mezcle pasado y futuro. Se emplea:
- separación train/test respetando el orden temporal,
- validación cruzada temporal mediante TimeSeriesSplit para comparar modelos y seleccionar hiperparámetros.

### 8.2 Ingeniería y selección de variables (feature engineering)

En la fase de ingeniería de variables se realizaron transformaciones acordes a lo visto en clase y a la naturaleza del problema:
- transformaciones logarítmicas y ratios para estabilizar escalas,
- creación de variables temporales derivadas,
- selección de variables basada en coherencia con el problema y rendimiento en validación.


### 8.3 Modelo base y modelos candidatos

Con el objetivo de abordar el problema de predicción del número de turistas desde una perspectiva de aprendizaje automático, se definió en primer lugar un modelo base Naive, utilizado como referencia mínima para evaluar la capacidad predictiva de los modelos más complejos.

A partir de este modelo base, se seleccionaron varios algoritmos candidatos ampliamente utilizados en problemas de regresión con datos tabulares, con el fin de comparar su rendimiento bajo un mismo marco de evaluación. Los modelos considerados fueron:

- Naive, como modelo de referencia.
- CatBoost, modelo basado en árboles de decisión y gradient boosting.
- XGBoost, otro modelo de boosting ampliamente utilizado.
- Random Forest, modelo basado en un conjunto de árboles de decisión entrenados mediante bagging.

La inclusión de varios modelos permite contrastar distintos enfoques de aprendizaje y asegurar que la elección final no dependa de un único algoritmo, sino de una evaluación empírica sobre los datos disponibles.

### 8.4 Métricas de evaluación y justificación

Se utilizaron:
- MAE: mide el error medio absoluto 
- RMSE: penaliza más los errores grandes, relevante en planificación turística donde grandes desviaciones son críticas.
Para la selección del modelo se prioriza RMSE, usando MAE como métrica complementaria.

### 8.5 Selección de hiperparámetros

Para la selección de los hiperparámetros de los modelos candidatos se utilizó un proceso de búsqueda sistemática mediante grid search, evaluando distintas combinaciones de parámetros con el objetivo de controlar la complejidad y mejorar el rendimiento de cada modelo.
Los hiperparámetros evaluados se adaptaron a las características específicas de cada algoritmo, incluyendo parámetros relacionados con la profundidad de los árboles, la regularización, el número de iteraciones o árboles, y el número de variables consideradas en cada división, entre otros.
Cada combinación de hiperparámetros se evaluó mediante validación cruzada temporal, calculando las métricas MAE y RMSE en cada partición. Este enfoque permite respetar la estructura temporal de los datos y proporciona una estimación más realista del comportamiento de los modelos en escenarios de predicción futura.

### 8.6 Comparación de resultados

| Modelo               |    MAE |   RMSE |
| -------------------- | -----: | -----: |
| Naive                | 0.1660 | 0.2210 |
| XGBoost              | 0.1173 | 0.1585 |
| CatBoost             | 0.1173 | 0.1575 |
| Random Forest (CV)   | 0.1130 | 0.1535 |

Una vez seleccionados los hiperparámetros óptimos, el modelo Random Forest se entrenó de forma definitiva utilizando los datos históricos disponibles hasta el año 2024 y se evaluó sobre el conjunto de test correspondiente al año 2025.
La evaluación se realizó empleando las métricas MAE y RMSE, así como una métrica adicional de precisión relativa basada en el error porcentual medio. Estas métricas permitieron valorar el rendimiento del modelo tanto en términos absolutos como relativos.
Los resultados obtenidos mostraron que el modelo Random Forest supera claramente al modelo Naive y presenta un rendimiento competitivo frente al resto de modelos candidatos, destacando especialmente por la reducción del error absoluto en la predicción del número de turistas.


### 8.7 Entrenamiento final y evaluación

Los resultados obtenidos en la fase de validación y evaluación muestran que el modelo Random Forest presenta el mejor rendimiento global en términos de MAE y RMSE, superando al modelo Naive y obteniendo resultados ligeramente mejores que los modelos basados en boosting (CatBoost y XGBoost).

Aunque inicialmente se esperaba que los modelos de gradient boosting ofrecieran un mejor comportamiento, especialmente en datos tabulares con variables categóricas, los resultados empíricos indican que Random Forest se adapta de forma especialmente eficaz a la estructura concreta del dataset utilizado. La combinación de variables temporales, retardos (lags) y variables derivadas proporciona al modelo información suficiente para capturar la dinámica del turismo sin necesidad de un enfoque secuencial más complejo. Por otro lado, CatBoost mantiene una ligera ventaja en términos de precisión relativa, si bien esta métrica puede verse influida por provincias con menor volumen de turistas.  
Al tener estas dudas optamos por realizar la evaluación final sobre test tanto con el catboost y con el Randomforest, y dado que el objetivo principal del trabajo es minimizar errores absolutos en la predicción del número de turistas, se priorizan las métricas MAE y RMSE como criterios de decisión.
En base a estos resultados, se decidió seleccionar el modelo Random Forest como modelo definitivo para la resolución del problema planteado, al ofrecer el mejor equilibrio entre precisión, estabilidad y capacidad de generalización temporal.

### 8.8. Predicción final y resultados obtenidos

Una vez seleccionado el modelo Random Forest como modelo final, se procedió a realizar la predicción del número de turistas para el verano de 2026, considerando los meses de junio, julio y agosto.

La predicción se llevó a cabo utilizando un enfoque autoregresivo, debido a la ausencia de valores reales de turistas en los meses previos del año 2026. En este procedimiento, la predicción obtenida para cada mes se utiliza como referencia para calcular las variables necesarias del mes siguiente, reproduciendo un escenario realista de predicción futura.

El modelo proporciona predicciones mensuales por provincia, que posteriormente se agregan para obtener una estimación del total de turistas del trimestre de verano. Los resultadosse pueden ver aqui:

### Predicción final – Verano 2026 (Random Forest)

**Tabla completa de resultados:** 

|  Provincias   |  Turistas	 |   Provincias	  |   Turistas	|  Provincias	 |  Turistas	|  Provincias	 | Turistas |

Illes Balears	     6.500.470	    Granada	        279.807	     Córdoba	      102.850	     Ciudad Real	  16.277
Barcelona	         3.705.947	    Cádiz	          249.819	     Castellón/
                                                               Castelló	      83.639	      Jaén	        15.646
Madrid	           1.654.982	    A Coruña	      231.807	     Huesca	        79.881	      Palencia	    15.331
Las Palmas	       1.308.586	    Pontevedra	    212.030	     Valladolid	    71.397	      Ourense	      14.839
Girona	           1.230.866	    Asturias	      168.547	     Lleida	        69.513	      Teruel	      12.707
Málaga	           1.205.963	    Burgos	        154.246	     Araba/Álava	  65.845	      Albacete	    11.563
Santa Cruz de 
Tenerife	         850.449	      Huelva	        153.593	     Badajoz	      65.335	      Ávila	        11.395
Alicante/Alacant	 842.533	      Salamanca	      135.852	     Lugo	          54.955	      Ceuta	        10.113
Tarragona	         755.516	      Zaragoza	      134.045	     Toledo	        54.650	      Melilla	      9.098
Sevilla	           525.578	      Almería	        134.004	     León	          54.012	      Cuenca	      8.275
Valencia/València	 517.749	      Murcia	        130.735	     La Rioja	      49.158	      Zamora	      8.208
Bizkaia	           302.156	      Cantabria	      126.536	     Segovia	      38.970	      Guadalajara	  8.042
Gipuzkoa	         286.572	      Navarra	        119.016	     Cáceres	      31.662	      Soria	        5.981


Las predicciones obtenidas permiten identificar las provincias con mayor volumen esperado de turistas en el verano de 2026, ofreciendo una herramienta útil para la planificación y toma de decisiones en el ámbito del turismo.

## 9. Conclusiones finales

Los resultados obtenidos muestran que el modelo Random Forest proporciona predicciones consistentes y precisas del número de turistas por provincia, mejorando de forma clara al modelo Naive y ofreciendo el mejor rendimiento global en términos de MAE y RMSE entre los modelos evaluados. Esta reducción del error absoluto es especialmente relevante en provincias con mayor volumen de turistas, donde pequeñas mejoras porcentuales suponen diferencias significativas en valores reales.

La predicción final del verano de 2026, realizada mediante un enfoque autoregresivo, permite estimar de forma realista la evolución mensual del turismo y obtener una estimación agregada del trimestre estival por provincia. Los resultados obtenidos muestran patrones coherentes con el comportamiento histórico del turismo, identificando las provincias con mayor concentración de visitantes y reforzando la plausibilidad de las predicciones.

En conjunto, el modelo seleccionado ofrece una base sólida para la estimación anticipada del turismo estival, proporcionando información útil para la planificación y la toma de decisiones en el ámbito turístico.


---

# CUNEF Universidad  
Proyecto académico realizado por:  
**Sánchez, Joan — Garrido, Mirna — Baquera, Mario**  
Asignatura: *Aprendizaje Automático*
