# Proyecto Análisis de Datos
## Introducción
- **En este análisis busca desarrollar estrategias efectivas para medir riesgos y así determinar precios para los seguros de automóviles.**
- **Se busca obtener precios rentables y competitivos, que se basarán en la incidencia de delitos relacionados con automóviles.**
## Relevancia de la estrategia
- **Es importante tener un análisis de estos datos, puesto que es fundamental para la aseguradora poder elegir precios adecuados para los clientes y no tener perdidas de capital para el negocio.**
- **¿Por qué?**
- **Las zonas con altos índices de robos de vehículos o autopartes incrementan el riesgo de pérdida para las aseguradoras, lo que a menudo resulta en primas más altas para los propietarios de automóviles en esas áreas.**
## Descarga de los datos
- **Se obtendrán los datos oficiales desde la página del gobierno: https://datos.gob.mx/busca/dataset/incidencia-delictiva-del-fuero-comun-a-nivel-municipal**
- **Se descargará el archivo que contendrá los datos de delitos por años, pero se usará el que tiene todos los años en un solo csv.**
## Creación de base de datos.
- **Se crearán las columnas que hay dentro del archivo, y luego se procederá a importarlo a la base de datos.**
- **Se hace un SELECT para comprobar que se importaron los datos exitosamente**
![image](https://github.com/user-attachments/assets/e27a0440-12a4-4ef9-a511-f98e66f57d21)
![image](https://github.com/user-attachments/assets/43680191-f896-48cd-b4c2-674b28a6b44e)
## Conexión a la base de datos (Python)
- **Se configura la conexión con pyodbc, el cuál es un módulo de Python que permite conectar a bases de datos ODBC. Está crea una capa de abstracción entre el código y el controlador, lo que facilita la comprensión y el mantenimiento del código.**
- **Leemos los datos en un DataFrame para poder manejarlos.**
- **Se cierra la conexión a la base de datos puesto que ya no se necesita. Dejar conexiones abiertas innecesariamente puede causar problemas de rendimiento.**
## Limpieza de datos
- **Hacemos la verificación en los datos, de tipo, valores únicos y si hay valores nulos.**
- **Para propósitos de este análisis no es necesario eliminar columnas ni buscar una correlación en los datos.**
## Transformación de datos
- **Para hacer un análisis completo, necesitamos obtener el total de delitos cometidos y juntar los meses en una sola columna.**
- **Por tanto, se procede a hacer esa unión de columnas con melt, y así obtener estas dos nuevas columnas que facilitan el análisis de delitos.**
## Análisis con serie de tiempo
![image](https://github.com/user-attachments/assets/e54ba12c-37cd-4248-b8c6-8c25d2c9e340)
- **Objetivo: Identificar patrones y tendencias en los delitos de 2015 a 2021 y predecir los delitos en 2022.**
- **Selección de Municipio: Guadalajara.**
- **Se selecciona este municipio, porqué, como se observa en la gráfica, es el municipio con mayor incremento en delitos relacionados con automóviles a lo largo de los años.**
- **El municipio es importante para analizar porque desde 2020 a 2021 no hubo una tendencia muy alta o muy baja de delitos, entonces se hace importante la necesidad a predecir si irá en aumento o se reducen los delitos para el 2022.**
- **Para hacer una predicción del año 2022 en el número de delitos, primero tenemos que analizar la tendencia que llevo el número de delitos a lo largo del tiempo.**
![image](https://github.com/user-attachments/assets/d343ef4b-dd38-4813-ac25-9ed7bbc65d4a)
## Predicciones
- ** Se elige el modelo ARIMA porque funciona muy bien en predicciones a corto plazo. También porque se pueden ajustar los parámetros de la auto regresión, promedio móvil y diferenciación para poder adaptarlo a mi serie de tiempo.
En este caso indicando un incremento en el número de delitos para el año 2022.
El error absoluto nos da 1268.72, lo cual es una precisión decente. 
![image](https://github.com/user-attachments/assets/3b9b55fb-b709-4aee-ac42-eca8f0763759)
![image](https://github.com/user-attachments/assets/7238738c-1a8e-40a4-abe4-13dace0ecc29)
## Clasificación de estados por peligrosidad (Clustering)
- **Objetivo: Clasificar los estados según su peligrosidad en 2021.**
- **En el siguiente histograma que muestra la distribución del total de delitos, tiene un importante sesgo a la izquierda y unos cuantos outliers que indican demasiados delitos.**
![image](https://github.com/user-attachments/assets/e0f39cb6-ff2b-4228-bf1c-b75c342797e8)
## Normalización 
- **Se ajustan los valores de Total Delitos a rango mucho más reducido.**
- **La forma de la distribución es igual a la original, sin embargo, esta normalización ha permitido que los datos de Total Delitos sean más fáciles de comparar al reducir las grandes diferencias en las magnitudes de los valores.**
![image](https://github.com/user-attachments/assets/7c7ccdbd-de62-4a73-aae0-b83dd589b4e7)
## Algoritmo K-Means y Método Silhouette
. **Buscamos determinar el número optimo de clusters para agrupar los datos del Total Delitos Normalizado, el resultado de usar el algoritmo de K-Means es el gráfico elbow, en el cuál elige el valor adecuado de k que indica el punto donde agregar más clusters no reduce la distorsión.**
![image](https://github.com/user-attachments/assets/77dbdf97-9e42-4c34-b7b3-902656f44d1b)
- **Aquí se muestra el número de Clusters elegidos (4). Se elige esta cantidad de Clusters porque se entiende bien el análisis de la agrupación.**
![image](https://github.com/user-attachments/assets/a4f5c057-0173-46b7-ac91-5d3be0944e60)
![image](https://github.com/user-attachments/assets/2a8dcd60-5fcd-4ab0-991b-31b5e8cea0f5)
- **En el cluster 1 se encuentra un único dato, el cual es “México” el cuál es el outlier con una gran cantidad de delitos, justificando su inclusión en un cluster propio.**
- **En el cluster 2 se encuentran 3 datos los cuales también son parte de los valores altos, pero sin llegar al nivel de delincuencia del cluster 1.**
![image](https://github.com/user-attachments/assets/e60b0520-0168-4533-8748-8c3bef1f1daa)
- **En el cluster 0 se encuentra el grupo con un total de delitos más moderados sin llegar a extremos.**
![image](https://github.com/user-attachments/assets/0c81aceb-d2b7-4ada-b059-c8ed74353155)
- **El cluster 3 nos muestra los totales de delitos más bajos.**
## Interpretación de resultados
- **Gracias a esta agrupación, podemos ajustar nuestras estrategias de precios y coberturas de forma precisa, basándonos en el riesgo real que presenta cada entidad. No solo se optimizan los costos operativos, sino que también, nos permite ofrecer un servicio más justo y adaptado a las necesidades y riesgos de cada estado.**
- **Estado más peligroso: México**
![image](https://github.com/user-attachments/assets/8a166078-e4fd-44e6-9b89-b7b8c8632219)
# Conclusión
**Los descubrimientos que tenemos en este análisis son:**
- **Entre 2015 y 2017, observamos un aumento significativo en el número de delitos. A partir de 2017 la tendencia cambia, con una disminución constante hasta estabilizarse entre 2020 y 2021.**
- **El modelo ARIMA predice un incremento moderado para el año 2022, esto solo con los datos que disponemos, puede haber otras variables como asuntos económicos o sociales que puedan cambiar la predicción.**
- **México se destaca por tener un nivel extremadamente alto de delitos, muy por encima del resto del país. Esto indica que en esta región debemos aumentar la tarifa, ya que el riesgo para los vehículos es mayor.**
- **Ciudad de México, Jalisco y Baja California tienen también niveles de criminalidad altos sin ser tan extremos como el estado de México. Se recomienda un enfoque de tarifas ajustadas con coberturas que incluyan protección contra robo y daño asociados, pero manteniendo una flexibilidad en la oferta.**
- **Lo que encontramos en el Cluster 0, son entidades con criminalidad moderada, para estas zonas se puede mantener unas tarifas más estándar, aunque con atención a ciertas áreas que pueden requerir ajustes específicos si los datos locales lo justifican.**
- **En el Cluster 3 encontramos los estados con delitos más bajos, lo que significa que el riesgo es menor, en estos estados podemos ofrecer tarifas más competitivas, lo que permite atraer más clientes y al mismo tiempo mantener la sostenibilidad de nuestros servicios debió al menor riesgo.**
## Mejoras en el análisis
- **Se requiere incorporación de variables adicionales como, factores socioeconómicos (tasa de desempleo, ingreso promedio, educación), tiempo y mapas de calor geoespaciales.**
- **Análisis en tiempo real. Establecer un sistema de monitoreo y ajuste de los modelos predictivos en intervalos regulares para reflejar cambios en las tendencias.**
- **Evaluación  de impacto de políticas públicas, como el aumento de patrullaje o instalación de cámaras de seguridad afectan las tasas de criminalidad.**
- **Trabajar junto con equipos de marketing y ventas para evaluar cómo las predicciones de criminalidad impactan en las decisiones de los clientes y ajustar las estrategias de seguro en consecuencia**




























