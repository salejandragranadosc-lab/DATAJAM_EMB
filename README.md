# DATAJAM_EMB

En Bogotá se observa que las decisiones de desplazamiento de peatones, ciclistas y conductores se basan principalmente en la ruta más corta o más rápida, sin considerar la exposición diferencial al hurto, a la siniestralidad vial y a la deficiencia de iluminación pública a lo largo de la malla vial. Esta situación afecta a la ciudadanía que se moviliza cotidianamente por la ciudad —con mayor incidencia en franjas nocturnas y en poblaciones con mayor percepción de inseguridad— durante el periodo 2023–2025, lo que sugiere una posible brecha en la seguridad y en el acceso a información pública integrada que permita elegir rutas más seguras y no únicamente más rápidas.

Justificación del problema: 
Las herramientas de ruteo de uso masivo optimizan tiempo o distancia, pero no incorporan la seguridad como criterio de decisión, pese a que la inseguridad y la siniestralidad vial son problemas públicos prioritarios en la agenda distrital. La información sobre hurto, siniestros viales e iluminación pública existe en formato abierto, pero se encuentra dispersa y desarticulada, lo que impide convertirla en un instrumento de decisión para la ciudadanía y la administración. El ejercicio es relevante porque integra estas fuentes en un mismo soporte espacial y las traduce en recomendaciones accionables. Afecta a toda la población que se moviliza por la ciudad, con énfasis inicial en peatones (perfil más vulnerable y foco del piloto) y extensión a ciclistas y conductores. Tiene mayor incidencia sobre quienes transitan en horarios nocturnos y sobre poblaciones que reportan mayor percepción de inseguridad, lectura que puede validarse con la Encuesta de Percepción Ciudadana (EPC) 2025.


Este repositorio contiene las operaciones geográficas realizadas para establecer rutas seguras a partir de datos abiertos.
El siguiente proceso refleja las acciones realizadas para identificar las rutas seguras en Bogotá, para ello se identificaron 4 datos abiertos que se encuentran en las siguientes URL:
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
