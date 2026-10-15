# Análisis de entregas en comercio electrónico

Proyecto de la asignatura Estadística Computacional para la
Toma de Decisiones (MCDI501), Magíster en Ciencia de Datos e
Inteligencia Artificial, Universidad Andrés Bello.

**Integrantes:** Marco Álvarez Araya y Ricardo Jaramillo.
**Docente:** Dr. Jean Paul Maidana González.

## Objetivo

Analizar las características de los envíos y su relación con
el cumplimiento del plazo de entrega, aplicando las técnicas
estadísticas y computacionales trabajadas en la asignatura.

## Problema de decisión

Determinar dónde concentrar recursos logísticos para reducir los
envíos fuera de plazo, cuantificando la incertidumbre de cada
conclusión y distinguiendo las asociaciones robustas de las que
dependen de la estructura del conjunto de datos.

## Datos

Se utiliza el dataset E-Commerce Shipping Data, disponible en:
https://www.kaggle.com/datasets/prachi13/customer-analytics

La variable Reached.on.Time_Y.N indica:
- 1: entrega fuera de plazo.
- 0: entrega a tiempo.

El archivo original no se modifica ni se incluye en el repositorio.

## Organización

- `notebooks/`: análisis y código en Python.
  - `s1-ecommerce-analisis.ipynb`: versión base del grupo.
  - `s1-ecommerce-analisis_ric.ipynb`: versión completa de la Formativa 1 y la Sumativa 1.
- `datos/`: archivos originales y datos de trabajo locales (no versionados).
- `resultados/tablas/` y `resultados/figuras/`: tablas (`tabNN_*.csv`) y figuras
  (`figNN_*.png`) generadas por el notebook y citadas en los informes.
- `documentacion/decisiones-metodologicas.md`: bitácora de decisiones y su evidencia.
- `entregas/`: informes de las evaluaciones (Word y PDF de control).

## Cómo ejecutar el análisis

1. Descargar o clonar el repositorio.
2. Crear un entorno con Python 3.13 e instalar las dependencias:
   `python -m pip install -r requirements.txt`
3. Guardar el dataset con el nombre `ecommerce-shipping.csv`
   dentro de `datos/originales/`.
4. Abrir `notebooks/s1-ecommerce-analisis_ric.ipynb` en VS Code
   o Jupyter y seleccionar el entorno de Python.
5. Ejecutar todas las celdas en orden. El notebook regenera las
   tablas y figuras de `resultados/`.

## Trazabilidad

Cada tabla y figura de los informes indica en su pie la sección del
notebook y el archivo de `resultados/` que la origina. El notebook fija
la semilla 42 y registra las versiones de las librerías en su sección 0.

## Avance

| Evaluación | Estado | Entregables |
|---|---|---|
| Formativa 1 | Evaluada | `entregas/formativa-1/informe-f1-ecommerce.docx` |
| Sumativa 1 | Completa | `entregas/sumativa-1/informe-s1-ecommerce.docx`; notebook `_ric` |
| Sumativa 2 | Pendiente | Validación con bootstrap y permutación, Monte Carlo y robustez |
| Sumativa 3 | Pendiente | Imputación, regresión logística y presentación |

**Hallazgo principal de la Fase 2:** el archivo contiene un bloque inicial
(ID ≤ 3.135) con 100 % de envíos fuera de plazo que concentra los descuentos
mayores a 10 y los pesos de 2.000 a 3.999 g. Varias asociaciones del conjunto
completo cambian al excluirlo (análisis de sensibilidad exploratorio): solo la
mayor tasa de retraso de los productos de importancia alta conserva dirección
y significancia nominal. Ninguna variable justifica por sí sola una
intervención logística con la evidencia disponible.

## Versión evaluada

La Sumativa 1 corresponde al tag `sumativa-1`; los enlaces del informe apuntan
a esa versión.
