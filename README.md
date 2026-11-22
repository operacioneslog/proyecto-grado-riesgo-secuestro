# Modelo de priorización de riesgo de secuestro en municipios de Colombia

Prototipo reproducible del Proyecto de Grado II de la Especialización en Ciencia de Datos y Analítica de la UNAD.

- Autor: Jorge Enrique Vargas Soto
- Asesor: Isaac Esteban Camargo Freile

El cuaderno estima, para cada municipio de Colombia y cada mes, la probabilidad de que se registre al menos un secuestro. Compara una regresión logística y un bosque aleatorio contra cuatro líneas base (frecuencia histórica, mes anterior, promedio móvil de 12 meses y azar), corrige las probabilidades por el efecto de los pesos de clase y clasifica los municipios en niveles de riesgo Alto, Medio y Bajo.

## Resultados principales (prueba, enero de 2024 a julio de 2026)

| Modelo | AUC-PR | Captura top-50 (%) |
|---|---|---|
| Regresión logística (modelo final) | 0,237 | 47,7 |
| Bosque aleatorio | 0,245 | 50,0 |
| Frecuencia histórica | 0,222 | 49,0 |
| Azar | 0,017 | 4,5 |

Con los umbrales fijados en validación (4,8 % y 1,5 %), el nivel Alto reúne unos 29 municipios por mes y captura el 36,1 % de los eventos. Alto y Medio juntos, unos 124 municipios por mes, capturan el 64,7 %.

## Datos

Los datos no se incluyen en el repositorio. Deben descargarse de las fuentes oficiales y guardarse en una carpeta llamada `Archivos Excel datos actualizados 18 de Agosto de 2026`, al lado del cuaderno:

| Archivo | Fuente |
|---|---|
| `SECUESTRO.xlsx` | Ministerio de Defensa Nacional, Datos y cifras del sector defensa (descargado el 18 de agosto de 2026) |
| `DIVIPOLA-_Códigos_municipios_20261003.xlsx` | DANE, catálogo DIVIPOLA |
| `MGN2025_MPIO_GRAFICO/MGN_ADM_MPIO_GRAFICO.shp` | DANE, Marco Geoestadístico Nacional 2025, nivel municipal |

Las rutas del cuaderno son relativas a esa carpeta. Si los archivos se guardan en otro lugar, hay que cambiar las rutas de las secciones 1.1, 1.5 y 1.6.

## Cómo ejecutarlo

```bash
pip install -r requirements.txt
```

Después se abre `Python_Notebook_Proyecto_Grado_II.ipynb` y se ejecutan todas las celdas en orden. La ejecución completa tarda unos cuatro minutos. La parte más lenta es el ajuste de hiperparámetros del bosque (sección 8.5).

Se usa la semilla 42 en todos los pasos aleatorios, así que los resultados se repiten. El cuaderno también genera las figuras del documento (`fig2_hechos_por_departamento.png`, `fig3_estacionalidad.png`, `fig5_particion.png`), las capas para ArcGIS (carpeta `mapas_arcgis`) y dos archivos intermedios del panel (`panel_base.pkl` y `panel_variables.pkl`).

## Estructura del cuaderno

1. Fuentes de datos y universo de análisis
2. Exploración de los datos y autocorrelación espacial (índice I de Moran)
3. Panel municipio-mes y variable objetivo
4. Variables
5. Partición temporal (entrenamiento 2011 a 2022, validación 2023, prueba 2024 a julio de 2026)
6. Modelos de referencia
7. Regresión logística
8. Bosque aleatorio y ajuste de hiperparámetros
9. Calibración y niveles de riesgo
10. Dependencia espacial de los errores y variables de vecinos
11. Evaluación final en el conjunto de prueba
12. Resumen de resultados
