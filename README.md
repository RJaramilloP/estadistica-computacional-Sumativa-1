# Incumplimiento de plazos en envíos de comercio electrónico

**Evaluación Sumativa 1 · Análisis exploratorio e inferencial**
MCDI501 Estadística Computacional para la Toma de Decisiones · Magíster en Ciencia de Datos e Inteligencia Artificial · Universidad Andrés Bello

**Integrantes:** Marco Álvarez Araya y Ricardo Jaramillo
**Docente:** Dr. Jean Paul Maidana González
**Fecha de entrega:** 14/10/2026

| Entregable | Archivo |
|---|---|
| Informe técnico (PDF, 8 páginas) | [`entregas/sumativa-1/informe-s1-ecommerce.pdf`](entregas/sumativa-1/informe-s1-ecommerce.pdf) |
| Notebook ejecutado | [`notebooks/s1-ecommerce-analisis.ipynb`](notebooks/s1-ecommerce-analisis.ipynb) |
| Tablas y figuras citadas en el informe | [`resultados/`](resultados/) |
| Bitácora de decisiones metodológicas | [`documentacion/decisiones-metodologicas.md`](documentacion/decisiones-metodologicas.md) |

---

## Objetivo general del proyecto

Determinar dónde concentrar recursos logísticos para reducir los envíos fuera de plazo, cuantificando la incertidumbre de cada conclusión y distinguiendo las asociaciones que se mantienen de las que dependen de la estructura del conjunto de datos.

### Pregunta de investigación

¿Qué características de los envíos se asocian con el incumplimiento del plazo de forma suficientemente estable para fundamentar prioridades de intervención logística, considerando la incertidumbre de las estimaciones y la composición del conjunto de datos?

## Alcance de la Sumativa 1

Esta entrega caracteriza el conjunto de datos, estima parámetros poblacionales con intervalos de confianza y contrasta hipótesis sobre la asociación entre características del envío y el incumplimiento. Incorpora un análisis de sensibilidad exploratorio frente a la estructura del archivo.

**Preguntas de esta etapa:**

1. ¿Qué proporción de los envíos llega fuera de plazo y con qué precisión puede estimarse?
2. ¿Difiere el peso medio entre envíos a tiempo y fuera de plazo?
3. ¿Se asocia la importancia del producto con el cumplimiento?
4. ¿Se mantienen esas asociaciones al excluir el bloque inicial del archivo?

## Datos

Se utiliza el conjunto [E-Commerce Shipping Data](https://www.kaggle.com/datasets/prachi13/customer-analytics) (Prachi13, Kaggle): 10.999 envíos y 12 variables. La variable de respuesta `Reached.on.Time_Y.N` se interpreta como:

- **1:** entrega fuera de plazo.
- **0:** entrega a tiempo.

**El CSV no se incluye en el repositorio.** Su licencia de redistribución no está documentada en la fuente y el archivo no se modifica durante el análisis. Para reproducir el análisis:

1. Descargar el archivo desde Kaggle (requiere iniciar sesión).
2. Guardarlo como `datos/originales/ecommerce-shipping.csv`.
3. Verificar que sea la misma versión analizada:

   ```bash
   shasum -a 256 datos/originales/ecommerce-shipping.csv          # macOS / Linux
   ```
   ```powershell
   Get-FileHash datos\originales\ecommerce-shipping.csv -Algorithm SHA256   # Windows
   ```

   Valor esperado: `dcdc3fd8f0507ec400174ef70d88d634321cc2ce8f4ea11bf4e8c847fdd325b3`

## Contenido del repositorio

```text
.
├── README.md
├── .gitignore
├── requirements.txt
├── datos/
│   ├── originales/        # CSV local (no versionado)
│   ├── procesados/        # reservado para fases siguientes
│   └── control/           # reservado para fases siguientes
├── documentacion/
│   └── decisiones-metodologicas.md
├── entregas/
│   └── sumativa-1/
│       └── informe-s1-ecommerce.pdf
├── notebooks/
│   └── s1-ecommerce-analisis.ipynb
└── resultados/
    ├── figuras/           # fig01–fig10 (.png)
    └── tablas/            # diccionario_variables.csv, tab01–tab18, tab12b y tab14b (.csv)
```

## Metodología

El notebook sigue el orden del informe; cada tabla y figura del informe indica en su pie la sección del notebook y el archivo de `resultados/` que la origina.

| Informe | Notebook | Procedimiento |
|---|---|---|
| §1 Preparación y carga | §1 | Controles de calidad, diccionario y clasificación de variables (Stevens, 1946), decisiones de tratamiento |
| §2.1 Estadística descriptiva | §2.1–2.4 | Tendencia central, dispersión, asimetría y curtosis; frecuencias absolutas y relativas; histogramas y barras |
| §2.2 Análisis bivariado | §2.5 | Correlación de Spearman (Pearson como referencia); tasa de retraso por categoría; comparación por grupos |
| §2.3 Estructura del archivo | §2.6 | Diagnóstico por `ID`; definición del subconjunto de sensibilidad (ID > 3.135) y sus salvaguardas |
| §3 Estimación de parámetros | §3 | IC al 95 %: normal (TCL) para medias, Wald para la proporción y Wilson como complemento |
| §4.1 Prueba 1 | §4.1 | t de Welch (peso según cumplimiento), Levene como complemento, d de Cohen |
| §4.2 Prueba 2 | §4.2–4.3 | χ² de independencia (importancia × cumplimiento), V de Cramér, RR, OR; z de dos proporciones (alta vs. resto) |
| §4.3 Sensibilidad consolidada | §4.4 | Repetición de los contrastes en ID > 3.135 y clasificación de cada resultado |
| §5 Informe de avance | §5 | Síntesis e insumos para las sumativas 2 y 3 |
| §6 Notebook y repositorio | §0 y §6 | Entorno, semilla y reproducibilidad |

Todas las pruebas usan α = 0,05 e informan tamaño del efecto. Los contrastes complementarios y la sensibilidad son exploratorios; sus valores p no se ajustan por multiplicidad.

## Reproducción del análisis

Requisitos: Python 3.13 y el CSV ubicado según la sección **Datos**.

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

**Windows (PowerShell)**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Luego, abrir `notebooks/s1-ecommerce-analisis.ipynb` en VS Code o Jupyter, seleccionar el entorno `.venv` como kernel y ejecutar todas las celdas (**Run All**). El notebook fija la semilla 42, registra las versiones de las librerías en su sección 0 y regenera todos los archivos de `resultados/`. Al ejecutarlo en otra máquina pueden variar los últimos decimales por diferencias de coma flotante, sin efecto sobre las cifras informadas.

## Resultados principales

- **Incumplimiento:** 6.563 de 10.999 envíos llegaron fuera de plazo (**59,67 %**; intervalo de Wald al 95 % **[58,75 %; 60,59 %]**; Wilson [58,75 %; 60,58 %]).
- **Estructura del archivo:** el bloque inicial (ID ≤ 3.135) tiene 100 % de envíos fuera de plazo y concentra los descuentos mayores a 10 y los pesos de 2.000 a 3.999 g.
- **Prueba 1 (peso, Welch):** los envíos fuera de plazo pesan en promedio 896 g menos (t = −29,27; p < 0,001; d = −0,57). En ID > 3.135 la diferencia se atenúa a 67 g (p = 0,065).
- **Prueba 2 (importancia, χ²):** asociación significativa pero muy débil (χ² = 12,21; gl = 2; p = 0,0022; V = 0,033). En ID > 3.135, p = 0,059.
- **Contraste alta vs. resto (z):** diferencia de 5,81 puntos porcentuales (p < 0,001); en ID > 3.135, 4,31 puntos (p = 0,036). Es el único contraste que conserva dirección y significancia nominal en ambos conjuntos.
- **Medio de transporte:** sin evidencia de asociación con el cumplimiento (χ² = 0,74; p = 0,690).

**Interpretación:** con la evidencia disponible, ninguna variable justifica por sí sola una intervención logística. La mayor tasa de retraso de los productos de importancia alta es una señal exploratoria que requiere validación.

## Limitaciones

- No se documentan la empresa, el período de observación ni el diseño muestral: no puede asegurarse representatividad ni extrapolar los resultados a otras operaciones.
- La independencia de las observaciones se asume y no puede verificarse por completo.
- El corte en el ID 3.135 se identificó después de observar los datos; la comparación con el subconjunto es exploratoria y no constituye una validación independiente.
- Se desconoce si el descuento, las llamadas y la calificación se registraron antes o después de la entrega (riesgo de fuga de información).
- La variable de respuesta indica incumplimiento, pero no la duración del retraso.

## Relación con otras entregas

- **Formativa 1:** aproximación inicial (proporción de incumplimiento y asociación con el medio de transporte). La Sumativa 1 amplía ese trabajo e incorpora la retroalimentación docente.
- **Sumativa 2:** validación de los intervalos con bootstrap (percentil y BCa), de las pruebas con permutación, estabilidad de correlaciones, simulación Monte Carlo y análisis de robustez frente al bloque inicial.
- **Sumativa 3:** imputación con faltantes inducidos de forma controlada (el archivo no tiene faltantes) y regresión logística, anticipando la separación casi perfecta que produciría el descuento.
