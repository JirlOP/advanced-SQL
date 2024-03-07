# advanced-SQL

This is a repository about advanced SQL course in kaggle

## Buils on

Python, [Intro to SQL](https://www.kaggle.com/learn/intro-to-sql)

## Learnign Platform

Kaggle

## Course Link

[https://www.kaggle.com/learn/advanced-sql](https://www.kaggle.com/learn/advanced-sql)

## References

[Google Cloud BigQuery Legacy](https://cloud.google.com/bigquery/docs/reference/legacy-sql)

---

## Content

1. **JOINs and UNIONs**
   - Combinación de múltiples filas y columnas de tablas.
     - `JOINs`: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`.
     - `UNIONs`: `UNION`, `UNION ALL`, `UNION DISTINCT`.
   - CTE + `JOINs`
   - Función `tail()`, `MIN()`, `TIMESTAMP_DIFF()`.
   - Exercicios y ejemplos de `JOINs` y `UNIONs`. Además de calculo de tiempo de respuesta de un sistema.

2. **Analytic Fuctions**
   - Introducción a las funciones analíticas.
     - `OVER`, `PARTITION BY`, `ORDER BY`.
   - `Window Frame`.
   - Tres tipos de funciones analíticas:
     - `Analytic Aggregate Functions`: `AVG()`, `SUM()`, `COUNT()`, `MIN()`, `MAX()`.
     - `Analytic Navigation Functions`: `LEAD()`, `LAG()`, `FIRST_VALUE()`, `LAST_VALUE()`.
     - `Analytic Numbering Functions`: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `NTILE()`.
   - Ejemplos
     - Contar número acumulado de viajes por día.
     - Calcular el inicio y fin de estacionamiento de bicis.
   - Ejercicios
     - Predecir la demanda de taxis.(`AVG()`)
     - Separar y ordenar viajes por área de la comunidad.(`RANK()`)
     - Tiempo entre viajes.(`LAG(), TIMESTAMP_DIFF()`)


3. **Nested and Repeat Data**


4. **Writing Efficiet Queries**
