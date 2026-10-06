# Trabajo Práctico Integrador - Pipeline ETL de Exportaciones del NEA

## Descripción

El objetivo de este proyecto es implementar un pipeline ETL (Extract, Transform, Load) utilizando Python para obtener, transformar y almacenar datos de exportaciones de las provincias del NEA argentino

El pipeline obtiene los datos desde una API pública, realiza distintas transformaciones y controles de calidad, y finalmente genera un dataset en formato CSV, un resumen en formato JSON y un archivo de log con el registro de cada ejecución.

## Fuente de los datos

Los datos provienen de la API de Series de Tiempo del portal de datos abiertos del Estado argentino (datos.gob.ar), utilizando información del INDEC.

Se utilizan datos de:

- Exportaciones por provincia y país de destino.
- Exportaciones por provincia y rubro.
- Provincias: Chaco, Corrientes, Formosa y Misiones.
- Período: 1993-2024.
- Unidad: millones de dólares FOB.

## Funcionamiento del pipeline

El proceso está dividido en tres etapas principales:

### Extract

Se conecta a la API pública y descarga los datos de exportaciones por destino y por rubro para las cuatro provincias.

### Transform

Los datos obtenidos son transformados para generar un dataset analítico. Entre las transformaciones realizadas se encuentran:

- Conversión de los datos de formato ancho a formato largo.
- Clasificación de los destinos según su región.
- Cálculo de la década correspondiente a cada año.
- Cálculo de la participación porcentual de cada destino.
- Cálculo de la variación interanual.
- Ranking de destinos por provincia y año.
- Identificación de los tres principales destinos.
- Incorporación del rubro principal y la participación de productos primarios.

### Load

Antes de guardar los resultados se realizan controles de calidad, como la verificación de la cantidad de filas y columnas, la detección de duplicados y el control de valores fuera de rango.

Finalmente se generan los siguientes archivos:

- `data/processed/exportaciones_nea.csv`
- `data/processed/resumen.json`
- `logs/pipeline.log`

## Instalación y ejecución

Para ejecutar el proyecto es necesario tener Python instalado.

Clonar el repositorio:

```bash
git clone https://github.com/juanigithub/tp-final2
```

Ingresar a la carpeta del proyecto:

```bash
cd tp-final2
```

Ejecutar el pipeline:

```bash
python src/main.py
```

Para ejecutar los tests:

```bash
python tests/test_transform.py
```

## Resultado

La ejecución del pipeline genera un dataset de 1.408 filas y 13 columnas, correspondientes a las exportaciones de las cuatro provincias del NEA entre 1993 y 2024.

## Hallazgo en los datos

Al observar el dataset generado se puede notar el crecimiento de China como destino de las exportaciones de Chaco. En 1993 se exportaron aproximadamente 0,31 millones de dólares a China, lo que representaba solamente el 0,20 % de las exportaciones provinciales. En 2024 el valor alcanzó los 110,93 millones de dólares, equivalentes al 27,61 % del total de la provincia, convirtiendo a China en el segundo destino de las exportaciones chaqueñas ese año. Además, entre 2023 y 2024 las exportaciones hacia China aumentaron un 46,36 %.

## Estructura del proyecto

```text
src/
├── config.py
├── extract.py
├── transform.py
├── load.py
└── main.py

data/
├── raw/
└── processed/
    ├── exportaciones_nea.csv
    └── resumen.json

logs/
└── pipeline.log

tests/
└── test_transform.py
```
