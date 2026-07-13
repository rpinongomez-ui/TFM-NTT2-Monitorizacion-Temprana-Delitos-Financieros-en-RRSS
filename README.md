# Monitorización temprana de fraude financiero en redes sociales

Trabajo Fin de Máster del **Máster Universitario en Big Data Science**, curso académico **2025–2026**.

**Autoras:** Reyes Piñón Gómez y Alicia Alcántara Troncoso  
**Tutora académica:** Stella Maris Salvatierra  
**Tutor de empresa:** Roque Alejandro Rodríguez Cabreja  
**Entidad colaboradora:** NTT DATA

---

## Descripción

Este repositorio contiene el código, los datos finales, las tablas, las figuras y los archivos LaTeX desarrollados para el Trabajo Fin de Máster **“Monitorización temprana de fraude financiero en redes sociales”**.

El proyecto analiza publicaciones de X relacionadas con fraude financiero en dos contextos geográficos:

- **Estados Unidos**, utilizado como contexto fuente.
- **España**, utilizado como contexto de destino para estudiar la transferibilidad de patrones.

El trabajo no se limita a fraudes cometidos exclusivamente por medios digitales. Analiza conversaciones publicadas en redes sociales sobre distintas modalidades de fraude financiero, aunque el hecho descrito se haya producido mediante llamadas, mensajería, inversiones, suplantaciones de identidad, compraventa u otros canales.

Los dos corpus se extrajeron directamente de X y corresponden al año 2025. El corpus de España **no procede de la traducción automática del corpus estadounidense**.

---

## Objetivos

1. Construir y depurar dos corpus comparables sobre fraude financiero en Estados Unidos y España.
2. Identificar temas recurrentes mediante modelado temático no supervisado.
3. Clasificar las publicaciones por modalidad de fraude mediante aprendizaje *zero-shot*.
4. Comparar la distribución, la evolución temporal y la confianza de clasificación entre ambos países.
5. Evaluar la transferibilidad al contexto de España de los patrones observados en Estados Unidos.
6. Generar tablas y visualizaciones reproducibles para la memoria del TFM.

---

## Metodología

El flujo de trabajo combina:

- extracción de publicaciones mediante la API v2 de X;
- filtrado geográfico y lingüístico;
- limpieza y normalización textual;
- eliminación de duplicados y control de concentraciones excesivas por usuario;
- modelado temático no supervisado mediante **BERTopic**;
- clasificación *zero-shot* mediante **`facebook/bart-large-mnli`**;
- aplicación de BART-MNLI a los corpus de Estados Unidos y España para mantener la comparabilidad;
- postcorrección trazable de un error sistemático detectado en el corpus español;
- análisis comparativo y generación automatizada de tablas y figuras.

Durante el desarrollo se probaron enfoques multilingües basados en mDeBERTa-XNLI y otras configuraciones experimentales. Estos experimentos se conservan por trazabilidad, pero no forman parte del *pipeline* definitivo.

---

## Construcción de los corpus

### Estados Unidos

```text
12.726 registros de la base inicial consolidada
→ 2.324 registros tras los filtros estructurales
→ 1.928 registros en el corpus final
→ 1.892 publicaciones clasificadas como relevantes
→ 36 publicaciones clasificadas como “No relacionado”
```

### España

```text
8.104 registros de la extracción inicial
→ 1.381 registros tras la depuración previa
→ 1.229 registros en el corpus final
→ 1.226 publicaciones clasificadas como relevantes
→ 3 publicaciones clasificadas como “No relacionado”
```

En el corpus de España se detectó un error sistemático: **170 publicaciones relacionadas con la estafa del “hijo en apuros”** habían sido asignadas a la categoría de seguros.

Estas publicaciones se reasignaron a **Phishing / robo de identidad** mediante una regla explícita y reproducible. La postcorrección modifica la categoría asignada, pero no el volumen total del corpus.

---

## Principales resultados

| Indicador | Estados Unidos | España |
|---|---:|---:|
| Registros del corpus final | 1.928 | 1.229 |
| Publicaciones relevantes | 1.892 | 1.226 |
| Publicaciones “No relacionado” | 36 | 3 |
| Confianza media de clasificación | 0,596 | 0,515 |
| Phishing / robo de identidad | 33,0 % | 62,3 % |
| Inversión / cripto | 15,2 % | 13,3 % |

Los resultados muestran que **Phishing / robo de identidad** tiene un peso especialmente elevado en España. En Estados Unidos, la distribución está más diversificada entre phishing, inversión y criptomonedas, esquemas Ponzi, empleo, romance y otras modalidades.

---

## Estructura del repositorio

```text
.
├── data/
│   ├── raw/
│   │   ├── scam_us_FINAL_v8.csv
│   │   ├── scam_us_FINAL_classified.csv
│   │   ├── scam_es_FINAL_v2.csv
│   │   └── scam_es_FINAL_classified_corregido.csv
│   ├── processed/
│   └── README.md
│
├── notebooks/
│   ├── 01_etl_extraction.ipynb
│   ├── 02_etl_cleaning.ipynb
│   ├── 03_nlp_classification.ipynb
│   ├── 04_dataframe_export.ipynb
│   ├── 05_etl_corpus_espana.ipynb
│   ├── 06_etl_cleaning_espana.ipynb
│   ├── 07_nlp_classification_espana.ipynb
│   ├── 08_postcorreccion_es.ipynb
│   ├── 10_analisis_comparativo.ipynb
│   ├── 11_bertopic_espana_stopwords.ipynb
│   ├── 12_visualizaciones_tfm.ipynb
│   └── experimentos/
│
├── docs/
│   └── overleaf/
│       ├── Chapters/
│       ├── figures/
│       ├── tables/
│       ├── bibliografia.bib
│       └── main.tex
│
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Descripción de los notebooks

| Notebook | Contenido |
|---|---|
| `01_etl_extraction.ipynb` | Extracción y consolidación inicial del corpus de Estados Unidos. |
| `02_etl_cleaning.ipynb` | Limpieza, filtrado estructural, control de duplicados y preparación del corpus estadounidense. |
| `03_nlp_classification.ipynb` | Modelado temático y clasificación del corpus de Estados Unidos. |
| `04_dataframe_export.ipynb` | Exportación y preparación de los resultados del corpus estadounidense. |
| `05_etl_corpus_espana.ipynb` | Extracción directa del corpus de España desde X. |
| `06_etl_cleaning_espana.ipynb` | Limpieza y depuración del corpus español. |
| `07_nlp_classification_espana.ipynb` | Modelado temático y clasificación BART-MNLI del corpus de España. |
| `08_postcorreccion_es.ipynb` | Postcorrección trazable de los casos de “hijo en apuros”. |
| `10_analisis_comparativo.ipynb` | Comparación de categorías, confianza y diferencias entre países. |
| `11_bertopic_espana_stopwords.ipynb` | Ajuste de BERTopic para España mediante tratamiento de palabras vacías. |
| `12_visualizaciones_tfm.ipynb` | Generación reproducible de las tablas y figuras finales utilizadas en la memoria. |

La carpeta `notebooks/experimentos/` conserva pruebas metodológicas y modelos descartados. No forma parte del *pipeline* definitivo.

---

## Datos incluidos

El repositorio contiene los cuatro CSV finales necesarios para reproducir los análisis y las visualizaciones:

- `data/raw/scam_us_FINAL_v8.csv`: corpus limpio de Estados Unidos.
- `data/raw/scam_us_FINAL_classified.csv`: corpus estadounidense clasificado.
- `data/raw/scam_es_FINAL_v2.csv`: corpus limpio de España.
- `data/raw/scam_es_FINAL_classified_corregido.csv`: corpus español clasificado y postcorregido.

Las extracciones intermedias completas no se incluyen en el repositorio.

Los archivos contienen textos y metadatos derivados de publicaciones reales de X. Su uso debe limitarse al marco académico autorizado y respetar las condiciones aplicables a la fuente original.

---

## Instalación

Se recomienda utilizar **Python 3.11**.

### 1. Clonar el repositorio

```bash
git clone https://github.com/rpinongomez-ui/TFM-NTT2-Monitorizacion-Temprana-Delitos-Financieros-en-RRSS.git
cd TFM-NTT2-Monitorizacion-Temprana-Delitos-Financieros-en-RRSS
```

### 2. Crear y activar un entorno virtual

En macOS o Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

En Windows:

```powershell
python -m venv .venv
.venv\Scripts\activate
```

### 3. Instalar las dependencias

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configurar las credenciales de X

Las credenciales solo son necesarias para volver a ejecutar los notebooks de extracción.

```bash
cp .env.example .env
```

Después debe añadirse el *bearer token* correspondiente en `.env`.

> El archivo `.env` contiene información sensible y no debe subirse al repositorio.

---

## Ejecución

Los notebooks deben ejecutarse desde la raíz del repositorio para que las rutas relativas funcionen correctamente.

Para abrir Jupyter:

```bash
jupyter lab
```

El *pipeline* completo sigue la numeración de los notebooks. Para reproducir únicamente las tablas y figuras finales a partir de los CSV versionados, basta con ejecutar:

```bash
jupyter notebook notebooks/12_visualizaciones_tfm.ipynb
```

Este notebook no requiere repetir la extracción desde X.

---

## Tablas y visualizaciones finales

El notebook `12_visualizaciones_tfm.ipynb` genera automáticamente salidas en formato PNG, PDF y LaTeX.

Las figuras se guardan en:

```text
docs/overleaf/figures/
```

Las tablas se guardan en:

```text
docs/overleaf/tables/
```

Entre las salidas finales se encuentran:

- pipeline metodológico en color y en blanco y negro;
- construcción y clasificación completa de los corpus;
- distribución comparada de categorías con porcentajes;
- evolución mensual de las categorías;
- postcorrección de “hijo en apuros”;
- principales tópicos BERTopic de Estados Unidos;
- principales tópicos BERTopic de España.

---

## Memoria del TFM

La memoria está escrita en LaTeX y se encuentra en:

```text
docs/overleaf/
```

Para compilarla localmente con `latexmk`:

```bash
cd docs/overleaf
latexmk -pdf main.tex
```

Las figuras y tablas generadas por el notebook 12 se incorporan directamente a la memoria desde las carpetas `figures/` y `tables/`.

---

## Reproducibilidad

Para reproducir las salidas finales:

1. clonar el repositorio;
2. crear el entorno e instalar `requirements.txt`;
3. comprobar que los cuatro CSV finales están en `data/raw/`;
4. ejecutar `notebooks/12_visualizaciones_tfm.ipynb`;
5. revisar las salidas generadas en `docs/overleaf/figures/` y `docs/overleaf/tables/`.

Las cifras agregadas de las figuras y tablas se calculan a partir de los CSV incluidos en el repositorio.

---

## Confidencialidad y uso

Este repositorio es de uso académico y se mantiene como **repositorio privado**.

No se concede autorización para copiar, redistribuir, publicar o reutilizar el código, los datos o la memoria fuera del marco autorizado del Trabajo Fin de Máster.

Todos los derechos quedan reservados a las autoras.