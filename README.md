# Analysis-EverPeak
EverPeak Analysis.ipynb
EverPeak Analysis.ipynb_
Introducción
Como analista de datos, tu objetivo es evaluar cómo la movilidad urbana se relaciona con la productividad económica en las principales ciudades latinoamericanas. Para ello trabajarás con datos reales de TomTom Traffic Index y OECD Cities, que deberás limpiar, combinar y analizar para identificar en qué ciudades conviene invertir en infraestructura de transporte.

Comentario General Iteración #1
Armando, quería dejarte aquí una apreciación general de tu proyecto para que a partir de allí nos vayamos punto por punto.

Respecto a tu trabajo en esta primera iteración, has mostrado tus conocimientos de la mejor forma, utilizando los metodos correctamente, realizando filtros de forma sencilla y trabajando con las librerias par graficar de la mejor forma. Solo quedan algunas recomendaciones de mejora que te servirán para tus entregas a futuro y corregir el desarrollo.

Haz doble clic (o ingresa) para editar

🧩 Paso 1: Cargar y explorar
Antes de limpiar o combinar los datos, es necesario familiarizarte con la estructura de ambos datasets. En esta etapa, validarás que los archivos se carguen correctamente, conocerás sus columnas y tipos de datos, y detectarás posibles inconsistencias.

1.1 Carga de datos y vista rápida
🎯Objetivo: Importar las librerías necesarias, cargar los archivos CSV en DataFrames y realizar una revisión preliminar para entender su contenido.

Instrucciones:

Importa las librerías pandas, numpy, seaborn y matplotlib.pyplot.
Carga los archivos usando pd.read_csv():
'/datasets/tomtom_traffic.csv'
/datasets/oecd_city_economy.csv `.
Guarda los DataFrames en las variables traffic y eco.
Muestra las primeras 5 filas de cada DataFrame.

[ ]
# importar librerías
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

[ ]
# cargar archivos
traffic = pd.read_csv('/datasets/tomtom_traffic.csv')
eco = pd.read_csv('/datasets/oecd_city_economy.csv')

[ ]
# mostrar las primeras 5 filas de traffic
traffic.head(5)


[ ]
# mostrar las primeras 5 filas de eco
eco.head(5)

Tip: Si no usas print() la tabla se vera mejor.

Comentario de la revisora Iteración #1
Buen trabajo con la importación de las librerías y datasets requeridos, al igual que con los métodos para la exploración inicial de los datos.

🧩Paso 2: Explorar, limpiar y preparar los datos
Antes de combinar los datasets, inspecciona su estructura, tipos de datos, columnas y valores faltantes. Anota las columnas que necesiten limpieza y luego estandariza los nombres de columnas.

2.1 Explorar la estructura y tipos de datos
🎯Objetivo: Identificar columnas con tipos incorrectos, distribución y nulos, anotar las columnas que requieren conversión.

Instrucciones:

Usa .info() para conocer la estructura de ambos DataFrames.
Muestra los primeros 3 renglones de cada DF.
Identifica si los detalles de cada DF estan bien o si requieren correcciones y escribe tus conclusiones en el bloque Markdown.
¿Hay columnas que requieren conversión?¿ Cuáles son? ¿Que tipo de dato ienen y cuál deberían de tener?
¿Hay datos ausentes en alguna columna?

[ ]
# Examinar la estructura de traffic
traffic.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1004464 entries, 0 to 1004463
Data columns (total 12 columns):
 #   Column                          Non-Null Count    Dtype  
---  ------                          --------------    -----  
 0   Country                         1004464 non-null  object 
 1   City                            1004464 non-null  object 
 2   UpdateTimeUTC                   1004464 non-null  object 
 3   JamsDelay                       1004464 non-null  float64
 4   TrafficIndexLive                1004464 non-null  float64
 5   JamsLengthInKms                 1004464 non-null  float64
 6   JamsCount                       1004464 non-null  float64
 7   TrafficIndexWeekAgo             1004464 non-null  float64
 8   UpdateTimeUTCWeekAgo            1004464 non-null  object 
 9   TravelTimeLivePer10KmsMins      1004464 non-null  float64
 10  TravelTimeHistoricPer10KmsMins  1004464 non-null  float64
 11  MinsDelay                       1004464 non-null  float64
dtypes: float64(8), object(4)
memory usage: 92.0+ MB

[ ]
traffic.head(3)

En la estructura del DF traffic, se observa que:

Las columnas UpdateTimeUTC y UpdateTimeUTCWeekAgo son de tipo objeto, lo mejor es convertirlos a tipo datetime -Country y City son tipo texto, lo mejor seria convertirlos a Object -No se encuentran Valores Nulos

[ ]
# Examinar la estructura de eco
eco.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 30 entries, 0 to 29
Data columns (total 7 columns):
 #   Column           Non-Null Count  Dtype 
---  ------           --------------  ----- 
 0   Year             30 non-null     int64 
 1   City             30 non-null     object
 2   Country          30 non-null     object
 3   City GDP/capita  30 non-null     object
 4   Unemployment %   30 non-null     object
 5   PM2.5 (μg/m³)    30 non-null     object
 6   Population (M)   30 non-null     object
dtypes: int64(1), object(6)
memory usage: 1.8+ KB

[ ]


En la estructura del DF eco, se observa que:

Las columnas City GDP/capita, Unemployment %, PM2.5 y Population son tipo object pero lo correcto seria cambiarlos a tipo float64
Las columnas anteriormente mencionadas deberan ser limpiadas (estandarizar nombre, Eliminar simbolos, y cambiar las comas por puntos)
No hay valores nulos
2.2 Renombrar columnas
🎯Objetivo: Estandarizar los nombres de columnas para evitar errores y facilitar la unión de los datasets.

Instrucciones:

Cambia los nombres de las columnas para que tengan el formato snake_case.
Country → country
UpdateTimeUTC → update_time_utc
Verifica que los cambios se hayan aplicado correctamente usando .columns.

[ ]
# Estandarizar los nombres de las columnas de traffic
#tu código aquí
traffic = traffic.rename(columns={"Country": "country", "City": "city", "UpdateTimeUTC": "update_time_utc", "JamsDelay": "jams_delay", "TrafficIndexLive": "traffic_index_live", "JamsLengthInKms": "jams_length_kms", "JamsCount": "jams_count", "TrafficIndexWeekAgo": "traffic_index_week_ago", "UpdateTimeUTCWeekAgo": "update_time_utc_week_ago", "TravelTimeLivePer10KmsMins": "travel_time_live_10km_mins", "TravelTimeHistoricPer10KmsMins": "travel_time_historic_10km_mins", "MinsDelay": "mins_delay"})
# verificar cambios
traffic.columns
Index(['country', 'city', 'update_time_utc', 'jams_delay',
       'traffic_index_live', 'jams_length_kms', 'jams_count',
       'traffic_index_week_ago', 'update_time_utc_week_ago',
       'travel_time_live_10km_mins', 'travel_time_historic_10km_mins',
       'mins_delay'],
      dtype='object')

[ ]
# Estandarizar los nombres de las columnas de eco
#tu código aquí
eco = eco.rename(columns={"Year": "year", "City": "city", "Country": "country", "City GDP/capita": "city_gdp_capita", "Unemployment %": "unemployment_pct", "PM2.5 (μg/m³)": "pm25", "Population (M)": "population_m"})
# verificar cambios
eco.columns
Index(['year', 'city', 'country', 'city_gdp_capita', 'unemployment_pct',
       'pm25', 'population_m'],
      dtype='object')
2.3 Corregir formatos numéricos y de fecha
🎯Objetivo: Asegurar que las columnas de fechas y valores numéricos estén en formatos correctos para permitir análisis, cálculos y comparaciones precisas.

Instrucciones:

Convierte las columnas de fecha de traffic a formato datetime. Haz el cambio a prueba de errores.
En el dataset eco, limpia los valores numéricos:
En city_gdp_capita: elimina separadores de miles (.) y reemplaza las comas (',') por puntos ('.') antes de convertir a tipo float.
En unemployment_pct: elimina el símbolo de porcentaje (%) y reemplaza las comas (',') por puntos ('.') antes de convertir a tipo float.
En population_m: reemplaza las comas (',') por puntos ('.') antes de convertir a tipo float.
Finalmente, crea una nueva columna llamada population multiplicando population_m por 1,000,000 para obtener la población total.
Haz clic para ver la pista

[ ]
# Convertir las columnas de traffic a tipo fecha con pd.to_datetime()
traffic['update_time_utc'] = pd.to_datetime(traffic['update_time_utc'], errors='coerce')
traffic['update_time_utc_week_ago'] = pd.to_datetime(traffic['update_time_utc_week_ago'], errors='coerce')

# verificar el cambio
traffic.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1004464 entries, 0 to 1004463
Data columns (total 12 columns):
 #   Column                          Non-Null Count    Dtype         
---  ------                          --------------    -----         
 0   country                         1004464 non-null  object        
 1   city                            1004464 non-null  object        
 2   update_time_utc                 1004464 non-null  datetime64[ns]
 3   jams_delay                      1004464 non-null  float64       
 4   traffic_index_live              1004464 non-null  float64       
 5   jams_length_kms                 1004464 non-null  float64       
 6   jams_count                      1004464 non-null  float64       
 7   traffic_index_week_ago          1004464 non-null  float64       
 8   update_time_utc_week_ago        1004464 non-null  datetime64[ns]
 9   travel_time_live_10km_mins      1004464 non-null  float64       
 10  travel_time_historic_10km_mins  1004464 non-null  float64       
 11  mins_delay                      1004464 non-null  float64       
dtypes: datetime64[ns](2), float64(8), object(2)
memory usage: 92.0+ MB

[ ]
# Limpia separadores y convierte columnas numéricas en eco
eco['city_gdp_capita'] = eco['city_gdp_capita'].astype(str).str.replace('.', '', regex=False).str.replace(',', '.', regex=False).astype(float)
eco['unemployment_pct'] = eco['unemployment_pct'].astype(str).str.replace('%', '', regex=False).str.replace(',', '.', regex=False).astype(float)
eco['population_m'] = eco['population_m'].astype(str).str.replace(',', '.', regex=False).astype(float)

# Calcula la población total en unidades absolutas (Multiplica * 1000000)
eco['population'] = eco['population_m'] * 1000000

# verificar el cambio
eco.info()
eco.head(3)

Comentario de la revisora Iteración #1
Bien hecho! Has comprendido la estructura y tipo de datos, has corregido la nomenclatura de las columnas en el formato snake_case y los formatos de números y fechas. Buen trabajo.

🧩Paso 3: Extraer año y filtrar
Extraer el año permite filtrar la información y trabajar solo con el período más reciente y relevante.

3.1 Extraer columna año y filtrar 2024
🎯Objetivo Identificar el año de cada registro y mantener solo los registros del 2024.

Intrucciones

Como el DataFrame traffic no tiene una columna de año, utiliza el atributo .dt.year sobre su columna de fecha para crear una nueva columna llamada year.
Filtra las filas donde el año sea 2024.
Utiliza .copy() para crear dos nuevos DataFrames (traffic_2024 y eco_2024) para evitar modificar el dataset original.

[ ]
# Extraer el año de las fechas en update_time_utc
traffic['year'] = traffic['update_time_utc'].dt.year

# Verificar cambio
print(traffic.head(3))
  country       city         update_time_utc  jams_delay  traffic_index_live  \
0     ARE  abu-dhabi 2025-01-13 04:01:30.001       650.7                36.0   
1     ARE  abu-dhabi 2025-01-13 03:46:00.000       540.4                30.0   
2     ARE  abu-dhabi 2025-01-13 02:46:30.000        71.8                 7.0   

   jams_length_kms  jams_count  traffic_index_week_ago  \
0            109.1       162.0                    30.0   
1            101.4       136.0                    27.0   
2             18.9        23.0                     6.0   

  update_time_utc_week_ago  travel_time_live_10km_mins  \
0  2025-01-06 04:01:30.000                   11.614767   
1  2025-01-06 03:46:30.001                   11.003180   
2  2025-01-06 02:46:30.000                    8.196278   

   travel_time_historic_10km_mins  mins_delay  year  
0                       10.265330    1.349437  2025  
1                       10.031544    0.971635  2025  
2                        8.196510   -0.000232  2025  

[ ]


Comentario de la revisora Iteración #1
Excelente trabajo en este caso, pudiste filtrar de forma adecuada los datos para el año 2024 haciendo uso de los métodos necesarios para ello.

🧩Paso 4: Analizar y resumir datos de movilidad
Como el dataset de tráfico contiene múltiples registros por ciudad. En esta parte, calcularás los promedios anuales por ciudad para simplificar el análisis y obtener una visión más clara de las tendencias generales.

4.1 Calcular promedios de tráfico por ciudad
🎯Objetivo: Obtener una vista consolidada del tráfico promedio por ciudad y año, para analizar patrones generales sin depender de datos diarios.

Instrucciones

Agrupa los datos por city, country y year.
Calcula el promedio solo de las métricas de tráfico más relevantes: como jams_delay, traffic_index_live, jams_length_kms, jams_count, mins_delay, y tiempos de viaje (travel_time_live_per_10kms_mins y travel_time_hist_per_10kms_mins).
Guarda el resultado como traffic_city_year_2024, mantén las columnas como variables (no índices).
Haz clic para ver la pista

[ ]


🧠 Momento de reflexión
¡Excelente trabajo hasta aquí!

Ahora que ya tienes los promedios anuales por ciudad, es momento de observarlos con atención.

Piensa:

¿Cuál crees que tiene el mayor tiempo promedio de tráfico?
¿Será una ciudad de Europa, de Latinoamérica o de otra región del mundo?
Para descubrirlo, ejecuta esta línea de código:

traffic_city_year_2024.sort_values(["jams_delay"], ascending=False)

🔍 Observa qué ciudad aparece en los primeros lugares.

¿Te sorprenden los resultados? , ¿Coinciden con lo que imaginabas?


[ ]
traffic_city_year_2024.sort_values(["jams_delay"], ascending=False)

La ciudad con el mayor tiempo promedio de tráfico es La ciudad de Mexico (mexico-city)

Comentario de la revisora Iteración #1
Excelente trabajo con groupby para agrupar por ciudad pais y año, al igual que la creacion de las columnas para las medias por indicador.

La verificación que hiciste para descubrir a CDMX como la ciudad con mayor promedio de trafico está perfecta.

🧩Paso 5: Unir movilidad y economía
Combinar datasets te permite analizar cómo se relacionan los indicadores económicos con los de movilidad.

5.1 Unir tráfico (tabla principal) con indicadores económicos
🎯Objetivo: Combinar la información de tráfico y economía en un solo DataFrame para analizar cómo las condiciones económicas se relacionan con la movilidad urbana.

Instrucciones

Selecciona solo las columnas relevantes de cada dataset (por ejemplo, variables clave de tráfico y de economía).
Usa .copy() al crear subconjuntos para evitar modificar el dataset original.
Une ambos DataFrames y define como claves de unión a city y year.
Mantén solo las ciudades y años presentes en ambos datasets.
Guarda el resultado en una nueva variable llamada merged y muestra las primeras 5 filas.
Haz clic para ver la pista

[ ]


Comentario de la revisora Iteración #1
Seleccionaste correctamente las columnas necesarias de ambos DataFrames y ejecutaste pd.merge(..., on=['city','year'], how='inner') como se solicitó. También realizaste un buen trabajo de verificación con .head().

🧩Paso 6: Visualización y análisis de relaciones
Ahora que tienes un dataset limpio y unificado, es momento de visualizar patrones. Los gráficos te ayudarán a entender cómo se relacionan las variables económicas con las de movilidad urbana.

6.1 Visualizar relaciones entre economía y tráfico
🎯Objetivo: Analizar visualmente la distribución y la relación entre indicadores de tráfico y economía en 2024, para identificar posibles patrones o tendencias generales entre ambas variables.

Instrucciones

Usa las librerías seaborn y matplotlib.pyplot para generar los gráficos.
Visualiza la distribución del tráfico (jams_delay) mediante:
Boxplot → para observar la media, mediana y detectar valores atípicos.
Visualiza la distribución de la economía (city_gdp_capita) mediante:
Histograma → para analizar la forma de la distribución y el valor promedio del PIB per cápita.
Finalmente, compara ambas variables, para observar si existe alguna relación entre ellas, haciendo un solo gráfico de barras donde aparezcan ambos indicadores.
Recuerda agregar título y etiquetas a los ejes de tus gráficos.
Observa y comenta los patrones, valores extremos o posibles relaciones que identifiques.
Tip: Dentro de los parentesis del boxplot, agrega showmeans=True para ver la media en el gráfico.


[ ]
# Crear boxplot para observar el comportamiento de los minutos de congestión JamsDelay

plt.figure(figsize=(8, 5))

# Crear el gráfico
sns.boxplot(
    y=merged["jams_delay"],
    showmeans=True
)

# Obtener promedio para mostrarlo en el título
mean_value = merged["jams_delay"].mean()

plt.title(f"Boxplot de JamsDelay (2024)\nPromedio: {mean_value:.2f}")
plt.ylabel("Minutos de congestión")
plt.show()


[ ]
# Crear histograma para ver la distribución de la economía (city_gdp_capita)

plt.figure(figsize=(8, 5))

sns.histplot(
    merged["city_gdp_capita"],
    bins=20,
    kde=True
)

# Obtener promedio para mostrarlo en el título
mean_gdp = merged["city_gdp_capita"].mean()

plt.title(f"Distribución del PIB per cápita (2024)\nPromedio: {mean_gdp:.2f}")
plt.xlabel("PIB per cápita")
plt.ylabel("Frecuencia")

plt.show()



[ ]

# Gráfico de barras para comparar jams_delay y city_gdp_capita por ciudad

merged.plot(
    x="city",
    y=["jams_delay", "city_gdp_capita"],
    kind="bar",
    figsize=(12, 6)
)

plt.title("Comparación de Jams Delay y PIB per cápita por ciudad (2024)")
plt.xlabel("Ciudad")
plt.ylabel("Valor")
plt.legend(["Jams Delay", "PIB per cápita"])

# Rotar etiquetas del eje X
plt.xticks(rotation=90)

plt.show()

Tip: Antes del plt.show() agrega el código plt.xticks(rotation=90) para rotar las etiquetas del eje X en 90 grados.

🧠 Reflexiona
Excelente trabajo llegando a esta etapa del análisis. Antes de avanzar, revisa tus gráficos, tómate un momento para pensar:

¿Las ciudades con mayor PIB per cápita también presentan más congestión?

¿O sucede lo contrario, o no existe una relación clara?

Escribe tus comentarios: No se observa una relación clara entre el PIB per cápita y la congestión vehicular. Algunas ciudades con mayor PIB pueden presentar niveles altos de congestión, pero esto no ocurre necesariamente en todas. Por lo tanto, el nivel económico por sí solo no parece explicar las diferencias en congestión. Sería necesario analizar otros factores, como población, infraestructura vial y número de vehículos, para comprender mejor este comportamiento.

Comentario de la revisora Iteración #1
Graficaste correctamente con sns.boxplot() para el tráfico y con sns.histplot() correctamente para la economia con etiquetas adecuadas. También graficaste correctamente en barras las variables jams_delay vs city_gdp_capita, pero si lo notas, hay una diferencia grande en terminos de los rangos de cada columna, de forma que jamsdelay se ve achatado en cero. Para ello debes usar un eje extra que permita visualizar bien los datos.

🧩Paso 7: Exportar y documentar resultados
En esta etapa final consolidarás todo tu trabajo: guardarás el dataset limpio y crearás un resumen que documente los resultados del proyecto.

7.1 Guardar dataset final
🎯Objetivo: Generar un CSV limpio, reproducible y con columnas relevantes para análisis posterior.

Instrucciones

Exporta el DataFrame merged con el nombre: ladb_mobility_economy_2024_clean.csv
Usa index=False para no incluir el índice.

[ ]
# Exporta el dataset final como CSV
merged.to_csv("ladb_mobility_economy_2024_clean.csv", index=False)
Para poder ver o descargar el archivo generado:
En el menú lateral que esta a la izquierda, ve hasta la parte de abajo, a la sección de Exportar dataset para más información.

✅ Entregables
Notebook .ipynb con todas las celdas (código + comentarios).
CSV final: ladb_mobility_economy_2024_clean.csv.
Resumen ejecutivo breve en Markdown (3–5 párrafos).
Comentario de la revisora Iteración #1
Bien hecho! Creaste el archivo final como CSV.

🧾 Resumen ejecutivo (plantilla)
Completa este resumen al finalizar el análisis. Mantén 3–5 párrafos cortos, claros y accionables.

Contexto & objetivo:

Responde la pregunta central del análisis: ¿qué relación existe entre la movilidad urbana (congestión, tiempos de viaje) y la productividad económica (PIB per cápita)?
El análisis de 2024 busca identificar si existe una relación entre las condiciones de movilidad urbana y la productividad económica de las ciudades. Se utilizaron variables como jams_delay, traffic_index_live, jams_length_kms, jams_count, mins_delay, los tiempos de viaje y city_gdp_capita.

Los resultados iniciales no muestran una relación clara y directa entre el PIB per cápita y la congestión vehicular. Las ciudades con mayor PIB per cápita no necesariamente presentan mayores niveles de congestión, por lo que existen otros factores urbanos que probablemente influyen en el comportamiento del tráfico.

Cobertura de datos:
El análisis se realizó para el año 2024. Los datos fueron agregados por city, country y year, obteniendo promedios anuales de las principales métricas de movilidad. Posteriormente, se realizó una unión INNER entre los datos de tráfico y economía mediante las variables city y year, conservando únicamente las ciudades presentes en ambos datasets.

Metodología (alto nivel):
Se realizó la limpieza y preparación de los datos, incluyendo la conversión de fechas para obtener el año y la selección de las variables relevantes. Los registros de tráfico fueron agrupados por ciudad, país y año y se calcularon promedios para las principales métricas de movilidad.

Posteriormente, se utilizaron subconjuntos .copy() de los DataFrames y se aplicó una unión de tipo INNER utilizando city y year para integrar los indicadores económicos con los de movilidad. Finalmente, se utilizaron gráficos de caja, histogramas y gráficos de barras para validar las distribuciones, identificar posibles valores atípicos y observar tendencias generales.

Hallazgos iniciales:
Los resultados muestran diferencias importantes en los niveles de congestión entre las ciudades. Por ejemplo, Bogotá presenta un nivel elevado de jams_delay en comparación con otras ciudades observadas. Sin embargo, los resultados no permiten establecer que un mayor PIB per cápita implique necesariamente mayor o menor congestión.

¿Existen anomalías u outliers que requieran una revisión adicional?

Sí. El boxplot de jams_delay permite identificar posibles valores atípicos y ciudades con niveles de congestión considerablemente superiores al resto. Estos casos deberían analizarse posteriormente considerando factores como población, infraestructura vial, número de vehículos y características particulares de cada ciudad.

Recomendaciones
Se recomienda realizar un análisis más profundo de las ciudades que presentan simultáneamente alta congestión y bajo PIB per cápita, ya que podrían representar oportunidades prioritarias para inversión en infraestructura de transporte. También se recomienda validar las fuentes de datos e incorporar variables adicionales, como población, número de vehículos, infraestructura vial y transporte público.

¿Qué ciudad : Bogotá, Lima o Buenos Aires o alguna otra en particular, muestra la mayor correlación significativa entre altos niveles de congestión vehicular y bajos indicadores de productividad económica, sugiriendo ser una ciudad prioritaria para inversión en infraestructura de transporte?

Bogotá destaca por presentar un nivel elevado de congestión (jams_delay), por lo que puede considerarse una ciudad de interés para un análisis posterior. Para determinar la ciudad prioritaria de manera estadísticamente sólida, sería necesario contar con múltiples observaciones por ciudad a lo largo del tiempo y calcular la correlación entre congestión y productividad económica.

Comentario de la revisora Iteración #1
Excelente conclusiones. Tu descripcion detallada de los hallazgos y tus propuestas de accion en terminos de infraestructura y validacion de fuentes, son muy buenas. Te felicito por tu desempeño.

Productos pagados de Colab
-
Cancela los contratos aquí
