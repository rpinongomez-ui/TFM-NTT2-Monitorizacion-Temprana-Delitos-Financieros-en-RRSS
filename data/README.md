# Datos del proyecto

Los datasets de tweets **NO se versionan en Git** por razones de:

- **Privacidad (RGPD):** contienen usernames, ubicaciones declaradas y textos personales identificables.
- **Términos de servicio de X (Twitter):** prohíben la redistribución de tweets en bloque fuera de la API. La práctica académica estándar es compartir solo IDs y permitir la rehidratación por parte de cada investigador con su propio acceso a la API.
- **Tamaño y rendimiento:** los CSVs (cientos de MB en algunas iteraciones) ralentizarían las operaciones de Git y consumirían cuota de almacenamiento del repositorio.

## Estructura local

En las máquinas de las autoras, los datos viven en:

```
data/
├── raw/         # CSVs en bruto descargados de la API (sin filtros)
├── processed/   # Corpus tras limpieza y filtros estructurales
└── interim/     # Resultados intermedios (BERTopic, clasificación zero-shot)
```

Todas estas carpetas están en `.gitignore`. Sólo se versiona este `README.md` y los archivos `.gitkeep` que mantienen la estructura de carpetas en el repo.

## Backup y acceso

- **Local:** `~/Documents/tfm_repo/data/raw/` en las máquinas de las autoras.
- **Copia de seguridad:** Google Drive privado compartido entre autoras y tutoras.
- **Acceso externo:** los datos no se distribuyen públicamente. Para acceso bajo solicitud, contactar a las autoras.

## Datasets generados durante el proyecto

A modo de inventario, los CSVs principales que produce el pipeline son:

| Archivo | Contenido | Notebook que lo genera |
|---|---|---|
| `scam_us_FINAL_raw.csv` | Tirada bruta con `place_country:US` (327 tweets) | `06_etl_FINAL.ipynb` |
| `scam_us_run2_raw.csv` | Tirada bruta sin filtro geográfico (~12.000 tweets) | `07_etl_FINAL_consolidado.ipynb` |
| `scam_us_CONSOLIDATED_dedup.csv` | Unión de ambas tiradas, deduplicada | `07_etl_FINAL_consolidado.ipynb` |
| `scam_us_CONSOLIDATED_clean.csv` | Tras filtros estructurales (~2.300 tweets) | `07_etl_FINAL_consolidado.ipynb` |
| `scam_us_FINAL_v8.csv` | Corpus final tras filtros anti-bot/no-EEUU (~1.900 tweets) | `08_limpieza_anti_bot_no_usa.ipynb` |

## Reproducibilidad

El corpus puede regenerarse ejecutando, en orden:

1. `notebooks/06_etl_FINAL.ipynb` — primera tirada con `place_country:US`.
2. `notebooks/07_etl_FINAL_consolidado.ipynb` — segunda tirada sin filtro geográfico y unión con la anterior.
3. `notebooks/08_limpieza_anti_bot_no_usa.ipynb` — limpieza final (anti-bot, no-EEUU, cap por usuario).

### Requisitos

- Acceso a la API de X v2 endpoint `/search/all`. Requiere tier Enterprise, Academic Research o DSA.
- Token de tipo *bearer* configurado en un archivo `.env` (ver `.env.example` en la raíz del repo).
- Entorno Python con las dependencias listadas en `requirements.txt`.

### Coste API estimado

La regeneración completa del corpus consume aproximadamente:

- Tirada 1 (con `place_country:US`): ~10-15 llamadas API.
- Tirada 2 (sin filtro geográfico): ~40 llamadas API.
- Total estimado: ~50-55 llamadas al endpoint `/search/all`.

## Política de uso interno

- Los datos sólo se utilizan en el marco de este Trabajo Fin de Máster.
- No se redistribuyen ni se cargan en servicios externos públicos.
- Cualquier publicación derivada (memoria, defensa, artículos) sigue las prácticas habituales: agregados, ejemplos anonimizados o referencias a IDs.
