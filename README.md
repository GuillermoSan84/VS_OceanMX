# VS_OceanMX
Metodología para obtener el promedio mensual del viento superficial del mar a partir de datos de Copernicus y su visualización en un geoportal
# Análisis de Vientos Superficiales del Océano (Copernicus Marine)

Procesamiento de datos de viento (Global Ocean Wind L4) para generar mapas de dirección y magnitud promedio mensual.

## 📋 Métodos
1. **Descarga de datos**: 
   - Fuente: [Copernicus Marine](https://resources.marine.copernicus.eu) (producto `WIND_GLO_WIND_L4_REP_OBSERVATIONS_012_006`).
   - Variables: `eastward_wind` y `northward_wind` (diciembre 2016).

2. **Procesamiento en QGIS**:
   - Cálculo del promedio mensual con la **Calculadora Raster**.
   - Generación de vectores de dirección con el plugin **SAGA GIS**.

3. **Creación de malla en ArcMap**:
   - Fishnet de 0.3° recortada a zona marina.
   - Unión espacial con los vectores de viento.

## 🛠️ Herramientas utilizadas
- QGIS 3.x + SAGA GIS
- ArcMap 10.x
- Python (para scripts de automatización básica).

## 📊 Resultados
![Mapa de dirección del viento](outputs/wind_direction_map.png)
*Mapa de vectores de viento (promedio mensual, diciembre 2016).*

## 🚀 Cómo replicar
1. Descargar datos de Copernicus Marine.
2. Ejecutar scripts en `/scripts/` (requiere QGIS y ArcGIS).
