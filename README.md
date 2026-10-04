# Predicción de crecimiento de adopción en protocolos blockchain a partir de fundamentales *on-chain*

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jhsebastian/prediccion-crecimiento-tvl-defi/blob/main/EDA_Entregable1.ipynb)

Proyecto de aula del curso **Modelos y Simulación de Sistemas II (Aprendizaje Supervisado)**, Departamento de Ingeniería de Sistemas, Universidad de Antioquia.

**Integrantes:** Michael Stiven Tabares Tobón, María Fernanda Atencia Oliva, Valeria Vásquez Barragán y Jhon Sebastián Martínez Soto.

## Problema

¿La información fundamental y la historia reciente de un protocolo de finanzas descentralizadas (DeFi), disponibles hasta la semana *t*, permiten anticipar si ese protocolo estará en el 10 % de protocolos con mayor crecimiento relativo del valor total bloqueado (TVL) en la semana *t+1*?

Se plantea como una **clasificación binaria supervisada** sobre un panel (protocolo, semana) construido con la API pública de DeFiLlama. La validación es temporal (entrenamiento 2022–2024, validación 2025 y prueba 2026) y la métrica principal es la PR-AUC.

## Contenido del repositorio

| Archivo | Qué es |
|---|---|
| `Entregable_I.pdf` | Reporte del Entregable I (plantilla IEEE Transactions). |
| `EDA_Entregable1.ipynb` | Notebook que reproduce los resultados del reporte: descarga los datos, construye el panel y las variables, y genera las cifras, tablas y figuras. Tiene las mismas secciones que el reporte. |
| `reporte/` | Fuente LaTeX del reporte: `main.tex`, `references.bib`, `resultados_eda.tex` y la carpeta `figs/`. |
| `requirements.txt` | Librerías de Python para ejecutar el notebook fuera de Colab. |

## Cómo reproducir los resultados

### En Google Colab (recomendado)

1. Abrir el notebook con el botón **Open in Colab** del inicio de esta página.
2. (Opcional) En la celda **B.0 Parámetros**, poner `USAR_DRIVE = True` para guardar los datos descargados en Google Drive y no tener que bajarlos otra vez.
3. Ir a *Entorno de ejecución → Ejecutar todas*. La primera ejecución descarga el historial de todos los protocolos y puede tardar entre 20 y 60 minutos. Si se interrumpe, al volver a ejecutar solo se descarga lo que falta.
4. Al terminar se descarga `entrega_overleaf.zip`, con `resultados_eda.tex` y las cuatro figuras del reporte.

### En un computador propio

Requiere Python 3.10 o superior y conexión a internet. La API de DeFiLlama es pública y no necesita clave.

```bash
git clone https://github.com/jhsebastian/prediccion-crecimiento-tvl-defi.git
cd prediccion-crecimiento-tvl-defi
python -m venv .venv
source .venv/bin/activate          # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook EDA_Entregable1.ipynb
```

## Cómo compilar el reporte

1. Crear un proyecto en Overleaf y subir el contenido de `reporte/`. También sirve subir `main.tex`, `references.bib` y los archivos de `entrega_overleaf.zip`.
2. Compilar con pdfLaTeX. Overleaf ejecuta BibTeX automáticamente.

Las cifras del texto se leen de `resultados_eda.tex` y las figuras de `figs/`. Si se vuelve a ejecutar el notebook, basta con reemplazar esos archivos y recompilar.

## Datos

- **Fuente:** API pública de DeFiLlama (`https://api.llama.fi`), con los endpoints `/protocols`, `/protocol/{slug}`, `/summary/fees/{protocol}`, `/summary/dexs/{protocol}`, `/overview/fees`, `/overview/dexs` y `/v2/historicalChainTvl`.
- **Los datos no se guardan en el repositorio.** El notebook los descarga en `datos/` y anota la fecha de descarga en `datos/meta.json`. DeFiLlama revisa sus series con el tiempo, así que una descarga hecha en otra fecha puede cambiar levemente las cifras.
- **Descarga usada en el reporte (01/10/2026, hora UTC):** 253 502 observaciones (protocolo, semana) de 3 348 protocolos en 246 semanas, de enero de 2022 a septiembre de 2026. Cada observación tiene 24 variables numéricas, 2 indicadoras de disponibilidad y la categoría del protocolo en *one-hot*. La clase positiva es el 10.0 % de las observaciones.

## Archivos que genera el notebook

| Archivo | Contenido |
|---|---|
| `datos/panel_eda.csv.gz` | Panel (protocolo, semana) sin imputar, con las 24 variables, el crecimiento del TVL y la etiqueta. |
| `datos/dataset_modelo.csv.gz` | Dataset imputado, recortado y codificado, listo para entrenar modelos (Entregable II). |
| `figs/*.pdf` y `figs/*.png` | Figuras del reporte. |
| `resultados_eda.tex` | Cifras del reporte en formato LaTeX. |
| `entrega_overleaf.zip` | `resultados_eda.tex` y las figuras en PDF, listos para subir a Overleaf. |
