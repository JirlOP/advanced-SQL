# JOINs and UNIONs

## Keywords

1. `JOINs` and `UNIONs`
   * Combinación horizontal y vertical de tablas.
   * **CTE** + `JOINs`
   * `dataframe.tail()`
   * Ejemplos de `JOINs` y `UNIONs`
   * `MIN()`, `TIMESTAMP_DIFF()`
   * Calcular tiempo de respuesta de un sistema.
   * Combinar múltiples tablas.

## Notes

Notas del jupyter notebook de la clase.

1. `JOINs` and `UNIONs`
   * `JOINs`: para unir dos tablas, se usa `JOIN` y se especifica el tipo de `JOIN` que se quiere hacer. Los tipos de `JOIN` son:
      - `INNER JOIN`: se unen las filas de ambas tablas que cumplen con la condición especificada en `ON`. Solo la intersección de ambas tablas.
      - `LEFT JOIN`: se unen las filas de la tabla de la izquierda(la principal) con las filas de la tabla de la derecha que cumplen con la condición especificada. Tabla principal más la intersección de ambas tablas.
      - `RIGHT JOIN`: se unen las filas de la tabla de la derecha con las filas de la tabla de la izquierda que cumplen con la condición especificada. Lo mismo que `LEFT JOIN` pero con la tabla de la derecha como principal.
      - `FULL JOIN`: se unen todas las filas de ambas tablas que cumplen con la condición especificada. Como una unión de teoría de conjuntos.
      - `CROSS JOIN`: se unen todas las filas de la tabla de la izquierda con todas las filas de la tabla de la derecha.
   * `UNIONs`: para unir columnas de tablas(mezcla los resultados en una misma columna). Las columnas tienen que ser del mismo tipo de dato. Los tipos de `UNION` son:
      - `UNION`: se unen las filas de ambas tablas y se eliminan los duplicados.
      - `UNION ALL`: se unen las filas de ambas tablas y no se eliminan los duplicados.
      - `UNION DISTINCT`: es lo mismo que `UNION`. Elimina los duplicados.

* `MIN()`: función de agregación que devuelve el valor mínimo de una columna.
* `TIMESTAMP_DIFF()`: función que devuelve la diferencia entre dos fechas.
  * Use: `TIMESTAMP_DIFF(end_time, start_time, unit)`

## Importantes

* En el jupyter notebook están los ejemplos de `JOINs` y `UNIONs`.

* Query y devuelve el resultado en un dataframe.

```python
union_result = client.query(union_query).result().to_dataframe()
```

* Número de resultados de un dataframe.

```python
len(union_result)
```

* Sacar el tiempo de respuesta de una pregunta de algún sistema.

```sql
SELECT MIN(TIMESTAMP_DIFF(a.creation_date, q.creation_date, SECOND))
```

* Seleccionar el valor mínimos de grupos de datos.

```sql
SELECT owner_user_id
      , MIN(creation_date) AS first_post
  FROM `bigquery-public-data.stackoverflow.posts_questions`
GROUP BY owner_user_id
```

* Combinar múltiples tablas en el archivo de ejercicios.

[Exercise File](exercise-joins-and-unions.ipynb)

## Exerxices

Aquí, usará diferentes tipos de JOINs de SQL para responder preguntas sobre el conjunto de datos de Stack Overflow.

### 1) How long does it take for questions to receive answers?

Investigar cuánto dura una pregunta es ser respondida.

* `MIN()`
* Hay un ejemplo de cómo sacar el tiempo de respuesta de una pregunta. Tanto la query como el código en python para visualizarlo.
* Razones por las que la query esta incorrecta:
  * Tiene respuestas **incorrectas** en la columna `time_to_answer`.
  * El porcetanje de respuestas respondidas es 100% lo cual es **incorrecto**.
  * Queremos que salgan las preguntas respondidas y sin responder, porque lo que queremos hacer un `LEFT JOIN` de la tabla de preguntas con la de respuestas respectivamente. La query actual esta usando un `INNER JOIN` que solo devuelve las preguntas que tienen respuestas.
* El ejercicio se resuelve con un `LEFT JOIN` de la tabla de preguntas con la de respuestas.

### 2) Initial questions and answers, Part 1

¿Es más común que los usuarios primero hagan preguntas o proporcionen respuestas? ¿Cuánto tiempo pasa después de registrarse para que los usuarios interactúen por primera vez con el sitio web?

* Tengo que responder que JOIN usar para unir las tablas de preguntas y respuestas, tal que, podamos utilizar las fechas de crear preguntas y respuestas, tanto como para ver el ID de todos los usuarios que crearon las preguntas y respuestas.

### 3) Initial questions and answers, Part 2

Buscar todos los usuarios creados en Stack Overflow en el 2019 y ver cuando hicieron su primero preguntar-respuesta.

* Se añade la tabla de `users` para ver la fecha de creación de los usuarios. Y se usan igual que antes las tablas de preguntas y respuestas.
* Tener cuidado al usar `FULL JOIN` porque nos puede devolver hasta las preguntas y respuestas que no tienen usuarios creados. En este caso se usa `LEFT JOIN` porque queremos ver las preguntas y respuestas que tienen usuarios creados.

### 4) How many distinct users posted on January 1, 2019?

* Para responder esto, sin que se repitan los usuarios, se usa `UNION DISTINCT` para unir las tablas de preguntas y respuestas. Además de usar un `WHERE` para filtrar la fecha específica en que fueron creados.

## Learnings
