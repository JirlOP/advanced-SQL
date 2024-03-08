
# Nested and Repeated Data

## Keywords

1. Complex data types
   * `Nested Data`
   * `Repeated Data`
   * Combinados
2. Examples
3. Less JOINs

## Notes

1. Los datos complejos son:
   * **Nested Data**: Es un tipo de dato que puede contener otros datos con formato `JSON`. Por ejemplo otro tabla.
     * **Una columna puede contener un objeto JSON con varios campos**.
     * Si vemos en el `Schema` de la tabla, veremos que la columna tiene un tipo de dato `RECORD` or `STRUCT`. Dentro de ese `RECORD` hay otros `SchemaField` que representan los campos del objeto JSON.
     * Para consultar de `Nested Data` se usa la notación de punto `.`. Por ejemplo: `SELECT column_name.nested_column_name`.
   * **Repeated Data**: Es un tipo de dato que puede contener una lista de valores.
     * Una columna con una lista de valores del mismo tipo. Por eso es `REPEATED`.
     * En el `SchemaField` del `Schema` de la tabla, veremos que `mode` es `REPEATED`.
     * La entrada de esta columna es un `ARRAY` de valores de 0 o más elementos.
     * Para consultar de `Repeated Data` se usa `UNNEST()`. Por ejemplo: `FROM table_name, UNNEST(column_name) AS name`.
   * Se pueden combinar los dos tipos de datos complejos y hacer una lista con objetos de diferentes campos.
     * En la columna `SchemaField`, veremos que `mode` es `REPEATED` y `type` es `RECORD`.
     * Por lo tanto se una `UNNEST()` para descomponer la lista y luego se usa la notación de punto `.` para acceder a los campos del objeto JSON.
   * Evadir `JOINs` caros.

## Examples

Se utiliza el dataset Google Analytics Sample que tiene datos sobre visitantes de **Google Merchandise Store**. Un e-commerce de Google. Tiene varios nested fields.

1. El primer ejemplo es sobre cómo visualizar dos columnas `RECORD` sacando un dato de cada una.
2. `NESTED` y `REPEATED` data, explora datos de estas columnas.

## Exercises

Los ejercicios son hechos con el dataset `GitHub Repos` que tiene datos sobre los repositorios de GitHub.

### 1) Who had the most commits in 2016?

Se trabaja con la tabla `sample_commits`. Cada entradda es un commit.

Escriba una consulta para encontrar las personas con más confirmaciones en esta tabla en 2016. Su consulta debe devolver una tabla con dos columnas. **commiter_name**: nombre de la persona que hizo commit. **num_commits**: número de commits que hizo en 2016.

Las personas con más commits deben aparecer primero.

#### Soluciones

* Hay que hacer un count por cada commiter name. El nombre de saca del RECORD `committer` que tiene el campo `name`.
* Para la fechas es lo mismo, se saca del RECORD `committer` que tiene el campo `date`.

### 2) Look at languages!

Se trabaja con la tabla `languages`. Cada fila corresponde a aun repo diferente.

Este dataset representa la barrita de lenguajes que aparece en los repositorios.

Tiene una columna `repo_name` que es el nombre del repositorio. Y una columna `language` que es el lenguaje del repositorio.

`language` es un campo `REPEATED` porque un repositorio puede tener varios lenguajes y `RECORD` porque tiene el nombre de lenguajes y el numero de bytes.

**Queremos saber cuantas filas hay en la tabla de ejmplo que sale en la imagen del ejercicio.**

#### Soluciones

* Acordarse de `UNNEST()` hace que la lista se desglose en tantas filas según hayan elementos en la lista. Con eso ya tenemos la respuesta.

### 3) What's the most popular programming language?

Con el mismo dataset de `languages` hay que averiguar lo que dice el título.

#### Soluciones

* `lenguage_name` es el valor principal que se quiere contar en cada fila(repositorio). Por lo tanto se hace un `GROUP BY` de `lenguage_name` y se hace un `COUNT()` de `lenguage_name`.

### 4) What's the most popular programming language in each year?

Básicamente escribir una query para averiguar en `'polyrabbit/polyglot'` cuál lenguaje se uso más y cuántos bytes se usaron.

#### Soluciones

* Sencilla mente ordenar por `bytes` los lenguajes del repositorio `polyrabbit/polyglot`.

## Aprendizajes

* `GROUP BY` y `ORDER BY` van después de `WHERE`.
* Se aprendió a hacer tipos de datos más complejos sin tener que usar otras tablas para lograrlo, además de haver búsqueda en las mismas.

### Ejercicio 1

* Las fechas al usar `EXTRACT()` de año, semana, o un número en específico, se vuelve un `INTEGER`.
