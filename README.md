# Proyecto Final B · Arroyo Capital: inversión y portafolio en la industria del cine

Programación Analítica II · Universidad de Montevideo · S2 2026 · Docente: Camila Palomeque

**Autor:** Timoteo Morelli (trabajo individual)

## Qué hace el proyecto

Arroyo Capital quiere colocar USD 500 millones en financiar películas durante tres años. Con la base
histórica de películas estrenadas entre 1980 y 2020 y el IPC de Estados Unidos, el notebook:

1. **Audita y limpia la base**: duplicados exactos, películas repetidas con datos distintos, centinelas
   de faltante (-1, 0, 999), presupuestos cargados en miles, categorías escritas de varias formas y el
   patrón de las películas sin presupuesto.
2. **Construye las variables**: presupuesto y recaudación en dólares de 2020, múltiplo
   recaudación / presupuesto, si recuperó la inversión (regla del 2,5x) y retorno neto estimado.
3. **Analiza el negocio**: evolución nominal contra real, retorno por género, franquicia y clasificación,
   relación con el puntaje de IMDb, y riesgo por segmento (probabilidad de perder, peor decil y
   segmentos dominados).
4. **Visualiza** los hallazgos con Matplotlib, Seaborn y un gráfico interactivo con Plotly.
5. **Propone una tesis de inversión**: tres grupos de películas, la asignación de los USD 500 millones con
   cantidad de proyectos, una simulación de 10.000 escenarios del portafolio, una prueba de estrés y una
   prueba fuera de muestra (2015-2020).

## Estructura

```
proyecto-final-morelli/
├── README.md
├── requirements.txt
├── .gitignore
├── datos/
│   ├── peliculas.csv         # base de películas (Moodle, carpeta B_Portafolio_de_cine)
│   └── ipc_eeuu.csv          # IPC de Estados Unidos, base 1982-84 = 100
├── notebooks/
│   └── proyecto_final_cine.ipynb
└── salidas/                  # lo genera el notebook: gráficos, tablas y base limpia
```

## Cómo reproducirlo

**1. Clonar el repositorio**

```bash
git clone https://github.com/timoteomorelli-rgb/proyecto-final-morelli.git
cd proyecto-final-morelli
```

**2. Crear el entorno e instalar las dependencias** (Python 3.10 o más nuevo)

```bash
python -m venv .venv
source .venv/bin/activate        # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**3. Correr el notebook de arriba abajo**

```bash
jupyter notebook notebooks/proyecto_final_cine.ipynb
```

y en Jupyter: *Kernel → Restart Kernel and Run All Cells*. También se puede correr sin abrirlo:

```bash
cd notebooks
jupyter nbconvert --to notebook --execute --inplace proyecto_final_cine.ipynb
```

**En Google Colab:** subir el notebook y crear una carpeta `datos` en `/content` con los dos CSV. El
notebook detecta solo que está en Colab para mostrar los gráficos de Plotly.

El notebook es uno solo y se corre en orden. Lee los datos de `datos/` y guarda todo lo que genera en
`salidas/`. La simulación usa una semilla fija (`SEMILLA = 2026`), así que da siempre los mismos números.

## Salidas que genera

| Archivo | Qué es |
|---|---|
| `evolucion_nominal_real.png` | Recaudación mediana por año en dólares corrientes y de 2020 |
| `distribucion_multiplo.png` | Distribución del múltiplo con promedio y mediana |
| `generos_ordenados.png` | Múltiplo mediano y probabilidad de perder por género |
| `presupuesto_vs_recaudacion.png` | Dispersión presupuesto contra recaudación (hexbin, escala log) |
| `mapa_calor_correlaciones.png` | Correlaciones entre puntaje, votos, presupuesto y retorno |
| `mapa_riesgo_retorno.html` | Gráfico interactivo de riesgo y retorno por segmento (Plotly) |
| `simulacion_portafolio.png` | Distribución del retorno simulado del portafolio |
| `peliculas_limpias_con_retorno.csv` | Base limpia con todas las variables construidas |
| `riesgo_por_segmento.csv`, `asignacion_fondo.csv`, `resumen_simulacion.csv` | Tablas de la tesis |

## Principales decisiones de limpieza

| Problema | Filas | Decisión |
|---|---|---|
| Duplicados exactos | 58 (0,75%) | Se borran |
| Películas repetidas con otro id y otro presupuesto | 14 (0,18%) | Se queda el id original |
| Presupuesto -1 o 0, recaudación negativa, duración 0 o 999, IMDb 0 | 115 en total | Pasan a NaN |
| Presupuesto cargado en miles | 35 (0,46%) | Se multiplica por 1.000 |
| Género, clasificación y país escritos de varias formas | 696 géneros reescritos | Se normalizan |
| Películas sin presupuesto o sin recaudación | 2.285 (29,8%) | Fuera del análisis de retorno (el faltante tiene patrón) |

## Datos

Base construida por la cátedra sobre el dataset público
[kaggle.com/datasets/danielgrijalvas/movies](https://www.kaggle.com/datasets/danielgrijalvas/movies), con
problemas de calidad agregados para uso docente. Arroyo Capital es un fondo ficticio.
