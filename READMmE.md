# Análisis ConnectaTel

## Objetivo del proyecto

Este proyecto analiza el comportamiento de los clientes de **ConnectaTel**, una empresa de telecomunicaciones en Latinoamérica, utilizando información registrada hasta 2024.

El objetivo principal es explorar, limpiar y analizar los datos para:

- construir un perfil estadístico de los clientes;
- identificar problemas de calidad y consistencia en los datos;
- analizar los patrones reales de uso de llamadas y mensajes;
- detectar valores atípicos (outliers);
- segmentar a los clientes por edad y nivel de uso; y
- obtener conclusiones accionables para apoyar estrategias de retención, fidelización y mejora de los planes.

## Datasets utilizados

El análisis utiliza tres archivos CSV:

| Dataset | Descripción |
|---|---|
| `plans.csv` | Información de los planes actuales: precio, minutos incluidos, GB incluidos y costos por excedente. |
| `users_latam.csv` | Información de los clientes: `user_id`, edad, ciudad, fecha de registro, plan y fecha de baja (`churn`). |
| `usage.csv` | Detalle del uso de los servicios: llamadas y mensajes, incluyendo fecha, duración y longitud cuando corresponde. |

En el notebook, los DataFrames se cargan con los nombres:

- `plans`
- `users`
- `usage`

## Etapas del análisis

### 1. Carga y exploración
Se cargaron los tres datasets y se revisaron sus primeras filas, dimensiones, tipos de datos y valores no nulos.

### 2. Identificación de problemas de calidad
Se analizaron:

- valores faltantes y su proporción;
- valores inválidos o sentinels;
- variables categóricas y sus valores únicos;
- posibles inconsistencias en identificadores y edad;
- formatos y años de las variables de fecha.

### 3. Limpieza y estandarización
Se aplicaron reglas de limpieza, entre ellas:

- reemplazo del sentinel `-999` en `age` por la mediana;
- reemplazo de `?` en `city` por valores nulos (`pd.NA`);
- conversión de fechas a formato datetime;
- identificación y tratamiento de fechas fuera de rango;
- análisis de los valores nulos de `duration` y `length` según el tipo de registro.

Los valores nulos de `duration` y `length` se conservaron porque dependen de `type`: la duración corresponde a llamadas y la longitud corresponde a mensajes de texto. Por lo tanto, esos nulos representan atributos que no aplican al registro correspondiente.

### 4. Perfil de uso por usuario
Se agregaron los registros de `usage` por `user_id` para obtener:

- `cant_mensajes`
- `cant_llamadas`
- `cant_minutos_llamada`

Posteriormente, estas métricas se combinaron con la información de `users` para construir `user_profile`.

### 5. Estadística descriptiva y visualización
Se calcularon estadísticas descriptivas de las variables de uso y se analizó la distribución porcentual de los planes.

También se construyeron histogramas para:

- `age`
- `cant_mensajes`
- `cant_llamadas`
- `cant_minutos_llamada`

Las distribuciones de mensajes, llamadas y minutos muestran principalmente un sesgo hacia valores altos (sesgo a la derecha), mientras que la edad presenta un comportamiento más uniforme.

### 6. Identificación y tratamiento de outliers
Se utilizaron boxplots y el método IQR para detectar valores extremos.

Se identificaron outliers hacia valores altos en:

- `cant_mensajes`
- `cant_llamadas`
- `cant_minutos_llamada`

Estos valores se conservaron porque pueden representar comportamientos reales de usuarios con consumo intensivo y no necesariamente errores de captura.

### 7. Segmentación de clientes
Se crearon dos nuevas variables:

**`grupo_uso`**
- `Bajo uso`: llamadas < 5 y mensajes < 5
- `Uso medio`: llamadas < 10 y mensajes < 10
- `Alto uso`: resto de los casos

**`grupo_edad`**
- `Joven`: edad < 30
- `Adulto`: edad < 60
- `Adulto Mayor`: resto de los casos

Se visualizaron las distribuciones de ambos segmentos mediante gráficos de barras.

### 8. Insight ejecutivo y recomendaciones
Los resultados se tradujeron en conclusiones orientadas al negocio, con énfasis en:

- el segmento predominante de clientes;
- patrones de consumo;
- usuarios de alto consumo;
- estrategias de retención y fidelización;
- oportunidades de migración del plan Básico al Premium;
- posibles mejoras de la oferta comercial.

## Principales hallazgos

- El segmento **Adulto** es el grupo etario más numeroso.
- El grupo de **Uso medio** concentra la mayor cantidad de clientes.
- La edad no parece ser el principal diferenciador del nivel de consumo.
- El comportamiento típico corresponde a un consumo moderado: alrededor de **5.5 mensajes, 4.5 llamadas y 23.3 minutos de llamadas** por usuario.
- Los valores extremos se concentran principalmente en las variables de uso, especialmente en `cant_minutos_llamada`.
- Los usuarios de alto consumo representan una oportunidad para estrategias de fidelización, complementos y posible migración a planes de mayor valor.
- El plan Básico representa una parte importante de la base y puede utilizarse como puerta de entrada, mientras que Premium puede orientarse a usuarios con mayores necesidades de consumo.

## Cómo ejecutar el notebook

### Opción 1: Google Colab

1. Descarga el archivo `S7 Version-Estudiante-Project-ConnectaTel (1).ipynb`.
2. Abre [Google Colab](https://colab.research.google.com/).
3. Selecciona **Archivo → Subir cuaderno** y carga el archivo `.ipynb`.
4. Asegúrate de tener disponibles los tres datasets:
   - `plans.csv`
   - `users_latam.csv`
   - `usage.csv`
5. En el notebook actual, los archivos se leen desde la ruta `/datasets/`. Si ejecutas el proyecto fuera del entorno donde esa ruta existe, ajusta las rutas de `pd.read_csv()` para apuntar a la ubicación de tus archivos.
6. Ejecuta las celdas en orden para reproducir el análisis.

### Opción 2: GitHub + Google Colab

1. Sube el notebook y los datasets a tu repositorio de GitHub.
2. Abre el notebook desde Google Colab o selecciona **Open in Colab**.
3. Verifica que las rutas de los CSV coincidan con la estructura del repositorio o del entorno de ejecución.
4. Ejecuta las celdas de principio a fin.

## Guía breve de reproducción

Para reproducir el proyecto:

1. Tener Python 3 y Jupyter Notebook o Google Colab.
2. Instalar, si es necesario, las librerías utilizadas:

```bash
pip install pandas numpy seaborn matplotlib
```

3. Colocar los tres CSV en una ubicación accesible para el notebook.
4. Abrir el archivo `.ipynb`.
5. Ejecutar las celdas en el orden presentado.
6. Verificar que las tablas y visualizaciones se generen correctamente.
7. Revisar el bloque final de **Insight Ejecutivo para Stakeholders** para consultar las conclusiones y recomendaciones.

## Estructura sugerida del repositorio

```text
connectatel-analysis/
├── README.md
├── S7 Version-Estudiante-Project-ConnectaTel (1).ipynb
└── datasets/
    ├── plans.csv
    ├── users_latam.csv
    └── usage.csv
```

## Autor

Proyecto de análisis de datos **ConnectaTel**.
