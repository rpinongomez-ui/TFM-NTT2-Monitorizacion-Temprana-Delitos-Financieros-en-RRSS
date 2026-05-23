# Datos del proyecto

> ⚠️ **REPOSITORIO PRIVADO — NO HACER PÚBLICO**
>
> Este repositorio contiene datasets con **datos personales de usuarios reales**
> (nombres de usuario, nombres de perfil, ubicaciones declaradas y texto íntegro
> de publicaciones). Por este motivo, el repositorio **debe permanecer privado**
> de forma permanente. Hacerlo público supondría una infracción del RGPD y de los
> términos de servicio de la API de X. El historial de Git conserva los archivos
> aunque se eliminen posteriormente, por lo que esta restricción es definitiva.

## Contenido de datos versionado

A diferencia de la práctica habitual (no versionar datos), en este repositorio
**privado** se incluyen los dos datasets finales del proyecto, a petición de la
dirección del TFM y para facilitar su evaluación:

| Archivo | Contenido | Filas |
|---|---|---|
| `data/raw/scam_us_FINAL_v8.csv` | Corpus limpio tras filtros estructurales, anti-bot y geográficos. Entrada del pipeline de NLP. | 1.928 |
| `data/raw/scam_us_FINAL_classified.csv` | Corpus final con tópicos (BERTopic) y clasificación tipológica (zero-shot BART-MNLI). **Dataset definitivo de análisis.** | 1.928 |

El resto de archivos de `data/` (CSV intermedios, checkpoints, iteraciones
descartadas) **no se versionan** — permanecen solo en local y en copia de
seguridad privada.

## Estructura local

```
data/
├── raw/         # CSV descargados y procesados
├── processed/   # Resultados derivados (ej. Excel de entrega)
└── interim/     # Resultados intermedios
```

## Backup

Además del repositorio privado, los datos se conservan en una carpeta privada
de Google Drive compartida entre las autoras y la dirección del TFM.

## Columnas del dataset definitivo

`scam_us_FINAL_classified.csv` incluye, entre otras:

- **Identificación y metadatos:** `tweet_id`, `created_at`, `text`, `username`,
  `user_location`, métricas públicas del tweet.
- **Topic modeling (BERTopic):** `bertopic_id`, `bertopic_keywords`.
- **Clasificación tipológica (zero-shot BART-MNLI):** `predicted_category`,
  `confidence_score`, `is_relevant`.

La columna `predicted_category` es la empleada como variable principal en el
análisis del TFM.

## Reproducibilidad

El corpus puede regenerarse ejecutando, en orden, los notebooks de `notebooks/`:

1. `01_etl_extraction.ipynb` — extracción del corpus desde la API de X.
2. `02_etl_cleaning.ipynb` — limpieza estructural, anti-bot y filtrado geográfico.
3. `03_nlp_classification.ipynb` — topic modeling y clasificación zero-shot.
4. `04_dataframe_export.ipynb` — exportación del dataframe final.

La carpeta `notebooks/experimentos/` contiene iteraciones alternativas exploradas
durante el desarrollo (clasificación con etiquetas refinadas y consolidación
híbrida), conservadas a efectos de trazabilidad metodológica pero no empleadas
en el resultado final.

### Requisitos

- Acceso a la API de X v2, endpoint `/search/all` (tier Enterprise, Academic
  Research o DSA).
- Token *bearer* configurado en `.env` (ver `.env.example`).
- Entorno Python con las dependencias del proyecto (ver `requirements.txt`).

## Política de uso

Los datos se utilizan exclusivamente en el marco de este Trabajo Fin de Máster.
No se redistribuyen fuera de este repositorio privado ni se cargan en servicios
externos de acceso público. Cualquier material derivado de difusión pública
(memoria, defensa, publicaciones) emplea datos agregados o ejemplos anonimizados.
