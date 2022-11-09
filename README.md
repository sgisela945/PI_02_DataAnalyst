# PI_02_DataAnalyst
![avión-1140x600](https://user-images.githubusercontent.com/104787036/200881547-2af86e70-7acc-4600-8a44-825e01248a57.jpg)
##  **Contexto**
La **Organización de Aviación Civil Internacional (OACI)**, organismo de la Organización de las Naciones Unidas, quiere investigar en profundidad los accidentes producidos desde inicios del siglo XX. Para ello, les solicita la elaboración de un informe y un dashboard interactivo que recopile tal información. 

La OACI únicamente cuenta con un dataset sobre datos de accidentes de aviones, pero insta a la consultora de datos -de la que forman parte- que intente cruzar esta información con otras fuentes de su interés. Esto con el objetivo de obtener mayor claridad y consistencia en los fundamentos del estudio.

## **Propuesta de trabajo**
A raíz de esta solicitud, nuestro Project Manager nos encarga una serie de tareas a cumplir: 

+ Realizar un EDA con el dataset provisto, junto con un reporte de calidad y diccionario de datos
+ Buscar y relacionar información relevante con los eventos
+ Crear una base de datos en un motor SQL e ingestar el csv procesado
+ Elaborar un dashboard e idear un storytelling con el objetivo de presentarlo ante la OACI
+ Adjuntar todo el trabajo en un repositorio de GitHub

## **Stack tecnológico:**

+ `Python`: EDA + transformaciones 
+ `SQL`: base de datos
+ `Power BI` -preferentemente- o `Streamlit`: dashboard
+ `GitHub`: con un README.md donde se elabore el informe

A modo de resumen, se realiza un primer acercamiento al dataset con Python. Allí, crearán un notebook que contendrá un análisis exploratorio y realizarán las transformaciones y el preprocesamiento que consideren pertinentes.

En una instancia posterior, se ingesta el dataset en un motor MySQL de su preferencia. Finalmente, la herramienta de visualización Power Bi empleada debe conectarse y tomar los datos desde ese motor.
- - -
### ***`PASOS REALIZADOS`***
#### ***`1. Exploración inicial del dataset`***
Al explorar este dataset vemos que posee 5008 filas y 18 columnas 


Sus columnas son: 'Unnamed: 0', 'fecha', 'HORA declarada', 'Ruta', 'OperadOR',
       'flight_no', 'route', 'ac_type', 'registration', 'cn_ln', 'all_aboard',
       'PASAJEROS A BORDO', 'crew_aboard', 'cantidad de fallecidos',
       'passenger_fatalities', 'crew_fatalities', 'ground', 'summary'


En la mayoría son columnas del tipo objeto, por lo que trabajaremos para convertirlas a sus unidades correspondientes

Podemos empezar diciendo que:
*  Hay que pasar a formato fecha: fecha, hora declarada
*  Hay que pasar a número (str/float): cn_ln,	all_aboard,	PASAJEROS A BORDO,	crew_aboard	cantidad de fallecidos,	passenger_fatalities,	crew_fatalities	ground,
*  Que existen muchos datos null que estan completos con '?'

Significado de las columnas

* Date - Date of accident- Fecha del accidente
* Time - Local time, in 24 hr. in the format hh:mm - Hora local, en 24 hr. en el formato hh:mm
* Location - Location of the accident- Ubicación del accidente
* Operator - Airline or operator of the aircraft- Aerolínea u operador de la aeronave
* Flight - Flight number assigned by the aircraft operator- Número de vuelo asignado por el operador de la aeronave
* Route - Complete or partial route flown prior to the accident- Ruta completa o parcial volada antes del accidente
* Type - Aircraft type- Tipo de aeronave
* Registration - ICAO registration of the aircraft- Matrícula OACI de la aeronave
* cn/In - Construction or serial number / Line or fuselage number- Número de construcción o de serie / Número de línea o de fuselaje
* Total Aboard - Total people aboard- Total de personas a bordo
* Passengers Aboard - Passengers aboard- Pasajeros a bordo
* Crew Aboard - Crew aboard- Tripulación a bordo
* Total Fatalities - Total fatalities- Total de muertes
* Passengers Fatalities - Passengers fatalities- Muertes de Pasajeros
* Crew Fatalities - Crew fatalities- Muertes de la tripulación
* Ground - Total killed on the ground- Total de muertos en el suelo
* Summary - Brief description of the accident and cause if known- Breve descripción del accidente y causa si se conoce

#### ***`2. ETL de accidentes`***

Comenzamos cambiando el nombre de las columnas por otros más adecuados:

'delete','date', 'time', 'location', 'operator', 'flight', 'route', 'type', 
'registration', 'cn/ln', 'total_aboard', 'passengers_aboard', 
'crew_Aboard', 'total_fatalities', 'passengers_fatalities', 
'crew_Fatalities', 'ground', 'summary'


##### ***2.1 Normalización de las palabras***
Cambiamos todos los textos a minúsculas y sacamos carácteres especiales 
Ahora si obtenemos un primer resúmen del dataset

![02Tablaresumen](https://user-images.githubusercontent.com/104787036/200884775-33a83bfe-0450-4e33-a9cb-3b639b0eed72.JPG)

Evaluamos la cantidad de datos efectivos que teníamos en cada columna

# Poner foto

##### ***2.2 Exploración inicial de fecha***

Vemos que las columnas date y time no tienen un correcto formato, por lo que nos impide utilizarlas, es por esto que hemos unificado las columnas en una sola que posea el formato de mes,día,año hora,minutos:'%B %d %Y %H:%M'
Hubo un inconveniente con uno de los horarios en que quedaba:october 04 1992 1:75, siendo 75 un número inválido de minutos por lo que se sustituyó por 'october 04 1992 00:00'
Finalmente se obtuvo el rango de fechas contemplado que es de  1908-09-17 17:18:00 a 2021-07-06 15:00:00
Quedando:
# PONER FOTOOO!

##### ***2.3 Exploración inicial de location***

 A través de la columna location y la librería de Nominatim pudimos obtener las coordenas de 2910 puntos de los accidentes siendo una muestra reperesentariva ya que el total es de 5008 valores


##### ***2.4 Exploración inicial de operator***
 
 Para las mismas luego de algunas transformaciones se obtuvo un ranquing de los operadores con mayor cantidad de accidentes desde 17/09/1908 a 06/07/2021, obteniendo:

# oner fot
 Luego se obtuvieron los resultados según la suma de fatalidades totales por operador
# poner foto
 Finalmente se obtuvo la cantidad de fatalidades por cantidad de veces que el operador había tenido un accidente. Quedando el ránking de la siguiente manera:
 # poner foto 

    Finalmente analizamos en qué casos los operadores son militares y en cúales miliraes, evaluemos la proporcion en el total y durante el transcurso deltiempo
# poner foto

##### ***2.4 Exploración inicial de 'total_aboard','passengers_aboard','crew_Aboard','total_fatalities','passengers_fatalities','crew_Fatalities','ground'***
Estas columnas fueron pasadas a formato int por tratarse de personas y se obtuvieron sus valores característicos quedando:
# poner foto
#### ***2.5 Exploración inicial de summary***

Luego de normalizar esta columna, sacarle las stopword y algunas palabras que no eran relevantes como :['city','de','los','crashed','crew','plane','aircraft']
# poner foto

Se obtuvo la frecuencia de repetición de las palabras, entre las que se encuentran mencionadas condiciones del piloto en más de 1100 casos, el motor en más de 1000 veces que representa un 20% y la más importante el clima en más de 620 veces que es un 13% aproximadamente de las veces y se reportaron situaciones de fuego 500 veces que representan un 10% aproximadamente

#### ***2.6 Exploración inicial de route***
Se repitió el proceso realizado en summary y se obtuvo la siguiente información
# Poner foto

Se puede observar que si bien se encuentra separada la palabra new york se encuentra en la ruta con mayor cantidad de accidentes, luego le sigue paris, luego londres, luego moscow, chicago, roma, los ángeles, miami, bogota, Sao Paulo , Cairo, méxico

#### ***`3. Información extra`***
Elegimos un dataset donde se ven la cantidad de vuelos totales según el año en el rando de 1970 a 2016

From https://data.worldbank.org/indicator/IS.AIR.DPRT?end=2016&start=1970&view=chart
