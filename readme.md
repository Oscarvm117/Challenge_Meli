# Challenge MercadoLibre VIS — Practicantes

**Autor:** Oscar David Vergara Moreno

**Fecha:** 24 de junio de 2026

**Fuente de datos:** World Bank Development Indicators (actualización: 2023-03-01)

**Período analizado:** 2000–2021

**Países:** 217 (excluye 49 agregados regionales del World Bank)

**Motor SQL:** DuckDB (in-process)


---

## Stack utilizado

- **Python 3.13**
- **DuckDB** — motor SQL en memoria
- **Pandas** — limpieza y transformación de datos
- **Matplotlib / Seaborn** — visualizaciones

---

## Instalación y ejecución

### 1 — Clonar el repositorio

```bash
git clone https://github.com/TU_USUARIO/challenge-meli-vis.git
cd challenge-meli-vis
```

### 2 — Crear entorno virtual

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3 — Instalar dependencias

```bash
pip install duckdb pandas openpyxl xlrd matplotlib seaborn jupyter ipykernel
```

### 4 — Registrar el kernel de Jupyter

```bash
python -m ipykernel install --user --name=challenge-meli --display-name "Challenge MeLi"
```

### 5 — Abrir el notebook

```bash
code .
```

Dentro de VS Code abrir `Notebooks/challenge_meli_vis.ipynb`,
seleccionar el kernel **Challenge MeLi** y ejecutar **Run All**.

---

## Fuente de datos

World Bank Development Indicators — actualización 2023-03-01

| Archivo | Indicador | Código |
|---|---|---|
| BASE_CELULAR.xls | Suscripciones a telefonía celular móvil | IT.CEL.SETS |
| BASE_INTERNET.xlsx | Personas que usan internet (% de la población) | IT.NET.USER.ZS |
| BASE_POBLACION.xls | Población total | SP.POP.TOTL |

---

## Decisiones de diseño

- **DuckDB sobre BigQuery:** motor SQL en memoria sin necesidad de
cuenta cloud ni configuración de servidor. Mismo poder analítico,
setup en segundos.
- **Wide → Long:** los archivos del World Bank vienen con una columna
por año. Se transforman a formato long antes de cargar a DuckDB
para poder escribir queries SQL estándar.
- **Agregados regionales excluidos:** el World Bank incluye 49 códigos
que son sumas regionales (AFE, ARB, EUU, etc.), no países reales.
Se excluyen con `WHERE income_group != 'Agregados'`.
- **NULLs conservados:** los valores nulos no se imputan. Los NaN en
2021 para internet corresponden a datos no publicados por el World
Bank al momento de la extracción, no a errores de procesamiento.