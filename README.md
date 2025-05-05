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
  # Paso 2: Promedio mensual con Calculadora Raster
eastward_mean = processing.run("qgis:rastercalculator", {
    'EXPRESSION': '(eastward_wind@1 + eastward_wind@2 + ...) / n',
    'LAYERS': ['eastward_wind.nc'],
    'OUTPUT': 'eastward_mean.tif'
})
3. **Creación de malla en ArcMap**:
   - Fishnet de 0.3° recortada a zona marina.
   - Unión espacial con los vectores de viento.
# Paso 3: Vectores de dirección con SAGA
processing.run("saga:gradientvectorsfromdirectionaldirectionalcomponents", {
    'X_COMPONENT': 'eastward_mean.tif',
    'Y_COMPONENT': 'northward_mean.tif',
    'STEP': 1,
    'SIZE_MIN': 25.0,
    'SIZE_MAX': 100.0,
    'AGGREGATION': 0,  # Mean value
    'STYLE': 0,        # Arrow centered
    'OUTPUT': 'wind_vectors.shp'
})
# arcgis_fishnet.py
import arcpy
arcpy.env.workspace = "path/to/data"

# Crear fishnet
arcpy.CreateFishnet_management(
    out_feature_class="fishnet.shp",
    origin_coord="-124 35",  # Esquina noroeste
    y_axis_coord="-124 34.7",
    cell_width=0.3, cell_height=0.3,
    number_rows=None, number_columns=None,
    corner_coord="-80 10"  # Esquina sureste
)

# Recortar con línea de costa
arcpy.Clip_analysis("fishnet.shp", "coastline.shp", "fishnet_marine.shp")

# Unión espacial con vectores de viento
arcpy.SpatialJoin_analysis(
    target_features="fishnet_marine.shp",
    join_features="wind_vectors.shp",
    out_feature_class="final_wind_grid.shp",
    join_operation="JOIN_ONE_TO_ONE",
    join_type="KEEP_ALL",
    match_option="INTERSECT"
)
## 🛠️ Herramientas utilizadas
- QGIS 3.x + SAGA GIS
- ArcMap 10.x
- Python (para scripts de automatización básica).

## 📊 Resultados
![Mapa de dirección del viento](https://github.com/GuillermoSan84/VS_OceanMX/blob/main/VS_Oceanos.jpg?raw=true)
*Mapa de vectores de viento (promedio mensual, diciembre 2016).*
Disponibles para visualización en el geoportal de la UNINMAR del Instituto de Ciencias del Mar y Limnología de la UNAM: http://uninmar.icmyl.unam.mx/geoportal#zoom=5&lat=-11320833.54&lon=2475744.62&layers=BOOT

## 🚀 Cómo replicar
1. Descargar datos de Copernicus Marine.
2. Ejecutar scripts en `/scripts/` (requiere QGIS y ArcGIS).
