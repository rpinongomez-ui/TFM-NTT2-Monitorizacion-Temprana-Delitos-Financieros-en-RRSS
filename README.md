# Monitorización Temprana de Fraude Financiero en Redes Sociales

Trabajo Fin de Máster — Máster Universitario en Big Data Science, curso 2024-2025.

**Autoras:** Reyes Piñón Gómez · Alicia Alcántara Troncoso  
**Tutora académica:** Stella Maris Salvatierra  
**Tutor de empresa:** Roque Alejandro Rodríguez Cabreja

---

## Sobre el proyecto

Este repositorio contiene el código y la memoria del TFM, cuyo objetivo es identificar patrones discursivos asociados al fraude financiero en X (Twitter) mediante técnicas de Procesamiento del Lenguaje Natural, y evaluar la transferibilidad de los patrones detectados en Estados Unidos al contexto español.

El *pipeline* combina:

- Extracción de tweets vía API v2 de X (endpoint `search/all`).
- Limpieza y preprocesamiento textual.
- *Topic modeling* no supervisado con BERTopic.
- Clasificación *zero-shot* con `facebook/bart-large-mnli`.
- Validación manual sobre muestra aleatoria.
- Traducción al español con modelos Marian (Helsinki-NLP).

---

## Estructura del repositorio

```
.
├── notebooks/                  # Notebooks Jupyter del pipeline
│   └── 01_etl_tweets_x.ipynb   # Extracción de tweets de X
├── src/                        # (futuro) módulos reutilizables en .py
├── data/                       # NO se sube a GitHub (ver .gitignore)
│   ├── raw/                    # CSVs en bruto recién descargados
│   └── processed/              # Datos limpios y enriquecidos
├── docs/
│   └── overleaf/               # Espejo local de la memoria en LaTeX
├── .env.example                # Plantilla de variables de entorno
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Puesta en marcha

### 1. Clonar el repositorio

```bash
git clone https://github.com/<usuario>/<nombre-repo>.git
cd <nombre-repo>
```

### 2. Crear el entorno

```bash
python -m venv .venv
source .venv/bin/activate          # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configurar credenciales

Copia el archivo plantilla y rellena tu *bearer token* de la API de X:

```bash
cp .env.example .env
# Edita .env y sustituye `tu_token_aqui` por tu token real
```

> ⚠️ **Importante:** `.env` está en `.gitignore` y nunca debe subirse a GitHub. Si en algún momento se filtra accidentalmente, revoca el token desde el panel de desarrollador de X y genera uno nuevo.

### 4. Ejecutar el notebook de extracción

```bash
jupyter notebook notebooks/01_etl_tweets_x.ipynb
```

El CSV resultante se guarda en `data/raw/scam_us_all_posts_2025.csv` (fuera del control de versiones).

---

## Memoria del TFM

La memoria está escrita en LaTeX y se mantiene sincronizada con Overleaf. El espejo local vive en `docs/overleaf/`. Para reconstruir el PDF localmente:

```bash
cd docs/overleaf
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

---

## Flujo de trabajo en pareja

- **Rama `main`**: contiene siempre código y memoria estables.
- **Ramas `feature/<nombre>`**: una por funcionalidad o sección, se mergean a `main` vía *pull request*.
- *Commits* con mensajes descriptivos en español.

```bash
git checkout -b feature/preprocesado
# ... trabajo ...
git add .
git commit -m "Añade limpieza de URLs y menciones en el preprocesado"
git push -u origin feature/preprocesado
```

---

## Licencia y confidencialidad

Repositorio **privado**. Todos los derechos reservados a las autoras.

No se concede ninguna licencia de uso, copia, modificación o distribución sobre el código ni sobre la memoria del TFM. El contenido se comparte únicamente con las personas con acceso autorizado al repositorio (autoras, tutoría académica y tutoría de empresa) en el marco del Trabajo Fin de Máster.
