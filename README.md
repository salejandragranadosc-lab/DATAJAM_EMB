# DATAJAM_EMB
Este repositorio contiene las operaciones geográficas realizadas para establecer rutas seguras a partir de datos abiertos.
El siguiente proceso refleja las acciones realizadas para identificar las rutas seguras en Bogotá, para ello se identificaron 3 datos abiertos que se encuentran en las siguientes URL:
Eje Vial POT Bogotá D.C: https://datosabiertos.bogota.gov.co/dataset/95cf10a5-3205-42d3-83c5-8a48abe59a9e/resource/cd0c70dc-b77d-4a5a-bca8-a3521966e5df/download/eje_vial.zip
Histórico Siniestros Bogotá D.C: https://services2.arcgis.com/NEwhEo9GGSHXcRXV/arcgis/rest/services/HistoricoSiniestros/FeatureServer/0
Alumbrado Público: https://serviciosgis.catastrobogota.gov.co/arcgis/rest/services/serviciospublicos/sistemaalumbradopublico/MapServer/0
Evaluación de seguridad nocturna para las mujeres año 2019: https://serviciosgis.catastrobogota.gov.co/arcgis/rest/services/mujeres/seguridad/MapServer/1

# Análisis espacial para encontrar las rutas más seguras
 
## Datos utilizados
 
- Evaluación de seguridad Nocturna
- Siniestros viales
- Alumbrado Publico

## Operaciones realizadas
Filtro de Percepciones Bajas y muy bajas con la capa de Evaluación de seguridad nocturna para las mujeres año 2019: 
 ### Unión espacial
 Union entre Evaluación de seguridad nocturna para las mujeres año 2019:  con percepciones baja y muy baja con Histórico Siniestros Bogotá D.C
Join attributes by location 
 
Resultado:
Puntos de Alta Inseguridad

 ### Análisis de proximidad

 Se generó una malla de 100*100 metros (que es la medida aproximada de una manzana regular en Bogotá) con los puntos de alta inseguridad.
Resultado:
cobertura con clasificación cada 100 metros cuadrados de seguridad por siniestros viales y por seguridad.

### Normalización de Información
 
Con la información de luminarias por UPZ se hace una normalización para establecer de manera adecuada cual es el indicador de cantidad de estructuras lumínicas por hectárea, posterior a eso se realiza el filtro de las UPZ que contienen más de 12 luminarias por hectárea (número apropiado para sectores urbanos según RETILAP).
 
Resultado:
polígonos con información de más alta iluminación o por lo menos que cumple con criterios técnicos para zonas urbanas.
 
### Limpieza de Información

En la malla resultante, que contiene valores de siniestros y resultados bajos en percepción de inseguridad se hizo limpieza de los Valores en 0 (zonas sin siniestros viales o zonas de seguridad baja y muy baja)
Resultado:
Cobertura con zonas de siniestros y baja y muy baja seguridad.
 
### Intersección
 
Intersección
 
Se omite de la malla resultante en el paso anterior, las UPZ con cumplimento de requerimientos en iluminación.
Resultado:
Cobertura con zonas de siniestros, baja, muy baja seguridad y baja iluminación.

### Selección
 
Selección por localización
Se realiza una selección inversa de las vías que no están sobre la cobertura creada.
Resultado:
la capa obtenida corresponde a las vías que no están en zonas establecidas como inseguras por diferentes causas y que se pueden tomar en caso de requerir un camino seguro.
