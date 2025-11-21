TRABAJOML:
El objetivo es realizar un análisis exploratorio de datos (EDA) sobre información turística de España, combinando:

- Datos de viajeros/pernoctaciones por provincia (EOH – Encuesta de Ocupación Hotelera)
- Datos de movimientos turísticos internos por provincia de destino (TMOV).

 1. Problema de negocio

El turismo es uno de los principales motores económicos de España y el poder conocer qué provincias reciben más turista, en qué épocas del año y cómo se comportan las pernoctaciones es clave para:

- Planificación de recursos (hoteles, transporte, etc).
- Marketing turístico
- Toma de decisiones públicas y privadas

En este trabajo estudiamos:

- ¿Cómo se distribuye el turismo en España por provincia y por temporada (invierno, primavera, verano, otoño)?
- ¿Qué provincias concentran más turistas y pernoctaciones en cada época del año?

A través de este trabajo podremos:
- Detectar comportamientos tanto territoriales como temporales

2. Origen de los datoss 


Se utilizan dos fuentes oficiales INE:

- EOH (Encuesta de Ocupación Hotelera). Datos mensuales de viajeros y pernoctaciones por provincia. 
  - Provincias
  - Viajeros y pernoctaciones
  - Periodo
  - Total 

- TMOV (Movimiento turístico de residentes). Datos de número de turistas por provincia de destino, desagregados por provincia de origen y periodo.  
  - Provincia de origen
  - CCAA y provincia de destino
  - Concepto turístico
  - Periodo
  - Total

Ambos datasets se descargaron en formato CSV desde fuentes oficiales (INE / Datos.gob.es) y se combinan para obtener una visión mas amplia del turismo por provincia y temporada