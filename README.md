

<h1 align="center" style="color:#F26522;">
  Análisis Exploratorio de Datos (EDA) aplicado al Sector Hotelero en España
</h1>

<h3 align="center" style="color:#003A46;">
  Proyecto académico — Aprendizaje Automático  
  CUNEF Universidad
</h3>

---

# 1. Integrantes del equipo

| Apellidos | Nombre |
|----------|--------|
| **Baquero** | Mario |
| **Garrido** | Mirna |
| **Sánchez** | Joan |

---

# 2. Introducción

El turismo es uno de los motores fundamentales de la economía española y un eje estratégico en la competitividad del país. Dentro del sector, las **pernoctaciones hoteleras** constituyen uno de los indicadores más relevantes para medir la demanda turística y planificar decisiones tanto del sector público como del privado.

Como analistas de datos, nuestro cometido es analizar rigurosamente esta información para extraer conclusiones significativas, detectar patrones y preparar el terreno para futuros modelos predictivos basados en aprendizaje automático.

Este proyecto corresponde a la **Primera Entrega de la Evaluación Continua** de la asignatura *Aprendizaje Automático*, centrada en el **Análisis Exploratorio de Datos (EDA)** y en la correcta formulación del problema de negocio.

---

# 3. Problema de Negocio

##  Contexto empresarial


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
> **Predecir el número de turistas del próximo verano para para cada provincia** 


El equipo de análisis ha sido encargado de:

> **Realizar un estudio detallado del comportamiento mensual del turismo hotelero en España, analizando patrones por comunidad autónoma, residencia, tipo de indicador y periodo, con el fin de extraer conclusiones útiles para la toma de decisiones y preparar el conjunto de datos para modelos predictivos futuros.**

El objetivo empresarial es disponer de un análisis sólido que permita:

- optimizar precios y capacidad,
- planificar campañas promocionales,
- orientar decisiones de inversión regional,
- y preparar modelos predictivos para futuros escenarios.

---

# 4. Origen y descripción del dataset

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

# 5. Justificación del dataset

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

#  6. Planteamiento científico (Aprendizaje Automático)

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

#  7. Metodología del EDA (Primera Entrega) y correcciones aplicadas

La primera fase del proyecto se centró en comprender los datos, validar su consistencia y extraer conclusiones accionables.

### 7.1 Análisis preliminar
- revisión de tipos, duplicados y consistencia por columnas,
- verificación de nulos y valores inconsistentes,
- análisis temporal para confirmar periodicidad mensual.
  

### 7.2 Limpieza y preprocesado
Para garantizar la calidad del análisis, se realizaron las siguientes intervenciones críticas en los datos:

- Tratamiento de "Total Nacional": Se identificó que el dataset original incluía filas agregadas bajo el concepto "Total Nacional". Tras entender mejor el dataset identificamos filas que correspondían al total nacional mensual, es decir, al conjunto de todas las provincias juntas, otras filas que se correspondían al total por comunidades agrupadas, y por ultimo el resto de filas siendo lo mayoritario del dataset que eran el total por cada provincia. Por esto, decidimos establecer Las columas de la siguiente manera:
      - En la columna Total Nacional se establece un booleano en el cual el 1 representa las filas sobre el total nacional mensual.
      - En la columna Comunidad_ciudad_autonoma decidimos establecer los valores de cada comunidad o ciudad autonoma, pero también añadir el valor "no aplica" cuando se estaría refiriendo al primer caso (total nacional mensual)
      - En la columna provincias, al igual que el anterior se añade el valor no "aplica" para referirnos en este caso al primer o segundo caso que hablamos antes(total nacional mensual o total por comunidades agrupadas).

- Reasignación de Ceuta y Melilla: En el dataset original, Ceuta y Melilla aparecían en ocasiones solo como "Comunidades" sin asignación provincial clara, o con formatos inconsistentes. Se imputaron como "Provincias" dentro de sus respectivas categorías para permitir una comparativa homogénea con el resto de provincias españolas. Ahora computan como unidades territoriales equivalentes al resto de provincias para el modelo.

- Conversión Temporal: Se transformaron las columnas Año y Mes en un objeto datetime para facilitar la visualización de series temporales y el ordenamiento cronológico correcto en los gráficos.

###  7.3 Detección de errores
- Nulos: Se verificó la existencia de nulos en la columna Provincias derivados de los agregados nacionales (corregido en el paso 7.2).
- Outliers: 
Un outlier (valor atípico) es una observación que se desvía drásticamente del comportamiento general de los datos. En el sector turístico, estos pueden ser de dos tipos:Outliers Reales (Información): Picos extremos de demanda en agosto o caídas a cero durante el confinamiento COVID. Estos NO deben eliminarse porque reflejan la realidad. Outliers de Error (Ruido): Fallos de registro o imputación incorrecta de datos que distorsionan las estadísticas. Se detectaron valores atípicos mediante el método Z-Score. Se observaron picos extremos que se correspondían con almeria y burgos unicamente y se decidió eliminarlos siguiendo lo establecido por el z score calculado.
Criterio Propio Aplicado: :Para distinguir entre "pico real" y "error", no utilizamos un simple Z-Score (que habría borrado los veranos), sino un sistema de doble validación (Intersección de Z-Scores). Un dato solo se considera ruido y se elimina si cumple simultáneamente estas dos condiciones: Anomalía Histórica (Z_1): El dato es extraño para la historia de esa misma provincia. Anomalía Contextual (Z_2): El dato es extraño comparado con el resto de su Comunidad Autónoma en ese mismo mes y año. Este enfoque es el único que tiene sentido estadístico para datos nuestros datos que son estacionales: Si solo usáramos el criterio 1, eliminaríamos sistemáticamente los meses de agosto de las zonas de playa (que están a más de 3 desviaciones de la media anual), perdiendo información crítica. Al añadir el criterio 2 ("Contexto"), salvamos esos datos: "Es cierto que Baleares tiene un pico altísimo en agosto (alerta 1), pero como toda la comunidad de Baleares también está alta en esa fecha (alerta 2 no se activa), el dato se valida como correcto y se mantiene".Solo eliminamos el dato cuando es excepcionalmente alto para la provincia Y ADEMÁS no cuadra con lo que está pasando en su región en ese momento.

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

- Existe **alta concentración de demanda** en ciertos meses, lo que requiere planificación reforzada.  
- Las comunidades presentan **perfiles turísticos muy diferenciados**, lo que sugiere políticas adaptadas.  
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
Se entrenó un modelo base y modelos candidatos:
- Modelo base: Naive, como referencia mínima razonable
- Modelos candidatos: CatBoost Regressor y XGBoost Regressor

(La comparación se realiza siempre con el mismo esquema de validación temporal y mismas métricas)

### 8.4 Métricas de evaluación y justificación
Se utilizaron:
- MAE: mide el error medio absoluto 
- RMSE: penaliza más los errores grandes, relevante en planificación turística donde grandes desviaciones son críticas.
Para la selección del modelo se prioriza RMSE, usando MAE como métrica complementaria.

### 8.5 Selección de hiperparámetros
La selección de hiperparámetros se realizó con búsqueda en listas de parámetros sobre validación temporal.
Se eligió este enfoque por ser transparente, reproducible y apropiado dado el tamaño del espacio de búsqueda y los recursos disponibles.

### 8.6 Comparación de resultados

| Modelo   |    MAE |   RMSE |
| -------- | -----: | -----: |
| Naive    | 0.1660 | 0.2210 |
| XGBoost  | 0.1184 | 0.1598 |
| CatBoost | 0.1173 | 0.1575 |

CatBoost obtiene el mejor rendimiento en ambas métricas y además permite tratar variables categóricas de forma nativa, simplificando el pipeline.


### 8.7 Justificación de no incluir más modelos

En el temario se revisaron otros algoritmos (regresión lineal, Random Forest, Gradient Boosting clásico). Se consideraron, pero no se entrenaron por dos motivos:
- en datos tabulares con no linealidades, los métodos de boosting modernos suelen dominar a alternativas clásicas en rendimiento,
- en un contexto temporal, modelos como Random Forest o regresión lineal aportarían menos valor comparativo, y el objetivo era comparar modelos representativos y relevantes sin redundancia.

La elección de CatBoost y XGBoost cubre adecuadamente la familia de métodos más competitiva para este tipo de dataset y permite una comparación sólida.


### 8.8 Entrenamiento final y evaluación
Una vez seleccionado el modelo final (CatBoost), se reentrena con la configuración elegida sobre los datos de entrenamiento definidos y se evalúa sobre el conjunto de test reservado, manteniendo la separación temporal.





## 9. Conclusiones fiales






## 10. Decisiones técnicas clave

A lo largo del proyecto se tomaron varias decisiones técnicas relevantes, alineadas con los contenidos vistos en la asignatura y con la naturaleza del problema:
- Respeto de la estructura temporal de los datos: la separación del dataset y la validación se realizaron sin mezclar pasado y futuro, utilizando esquemas de validación temporal para evitar data leakage y obtener una evaluación realista.
- Uso de un target transformado: se optó por trabajar con una transformación logarítmica de ratios (log_ratio) para estabilizar la varianza, reducir asimetrías y mejorar el comportamiento de los modelos de regresión.
- Selección de modelos de boosting avanzados: se priorizaron CatBoost y XGBoost frente a algoritmos más simples, al ser métodos especialmente adecuados para datos tabulares con relaciones no lineales y heterogeneidad territorial.
- Tratamiento diferenciado de outliers: se distinguieron valores atípicos reales de errores de registro mediante un enfoque contextual, evitando eliminar picos estacionales que contienen información relevante para el problema de negocio.

## 11. Limitaciones y trabajo futuro

Aunque el modelo desarrollado ofrece resultados satisfactorios, presenta algunas limitaciones. En primer lugar, el enfoque se basa exclusivamente en datos históricos del propio sector hotelero, sin incorporar variables externas como climatología, eventos especiales o indicadores macroeconómicos. Además, el modelo no es un modelo temporal puro, por lo que no captura explícitamente dependencias secuenciales de largo plazo.
Como líneas de trabajo futuro, se propone la incorporación de variables exógenas, la exploración de modelos específicos de series temporales o redes neuronales, y la ampliación del sistema hacia un entorno de predicción continua que permita generar alertas tempranas para la planificación turística.

