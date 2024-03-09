# Writting Effcient Queries

## Keywords

1. Query optimizer
   * `show_amount_of_data_scanned()`
   * `show_time_to_run()`
2. Strategies
3. Efficiency examples
   * `DISTINCT`
   * Doble encapsulamiento con `WITH ... AS`

## Notes

1. Para optimizar las query que corren múltiples veces como una pagina web o una query que busque en un dataset muy grande, se puede usar `Query optimizer`.
   * Las mayorias de databases tienen un `Query optimizer` que se encarga de optimizar las querys.
   * Pero también se puede hacer otros trucos para optimizar las querys.
     * `show_amount_of_data_scanned()`: Muestra la cantidad de datos que se escanea en una query.
     * `show_time_to_run()`: Muestra el tiempo que se demora en correr una query.

2. Buenas prácticas para optimizar querys:
   1. Solo seleccionar las columnas que se necesitan.
   2. Lee menos datos.
      * Por ejemplo si hay una relación 1:1 de dos columnas, usar solo una reduce el tiempo de lectura y el total de datos leídos.
      * Si en la misma tabla hay `ID` y `NAME` que se puede clasificar como lo mismo, solo usar uno de ellos.
   3. Evade N:N JOINs.
      * Hacer funciones con `WITH ... AS` para reducir la relación de datos a enlazar.

## Examples

Dos ejemplos que cuentan el número de commits distintos y el número de archivos en varios repositorios de GitHub.

* Se hace uso de `DISTINCT` para contar el número de distintos commiters. `DISTINCT` se usa para contar el número de distintos valores en una columna.
* Se hace uso de una doble función con `WITH ... AS`. Esto para sacar dos tablas con la información necesaria de archivos y commiters.
* La query de arriba dura más porque hace un `JOIN` relación N:N. Básicamente en la segunda parte se formaron primero las tablas con los datos necesarios y luego se hizo el `JOIN`. O sea, se redujo a una relación de datos a enlazar más baja siendo 1:1.
* Además buscar por repositorios específicos, separadamente, reduce el tiempo de búsqueda. Porque se unen menos datos al hacer `JOIN`.

## Exercises

### 1) You work for Pet Costumes International.

Tengo que hacer 3 querys que funcionen las 3, pero solo puedo optimizar una de ellas. Tengo que escoger cual puede ser mejor optimizada.

1. Un software que devuelve resultados a los empleados de una empresa de envíos. Hay 3 tablas: `orders`, `shipments` y `warehouseLocation`. De esas tablas se saca la info para los empleados, de que cosa enviar y dónde.
2. Es una sola tabla de reviews de clientes. Sacar una lista de todos los clientes que han dejado reviews.
3. GPS en el collar de perros que manda la ubicación a una base de datos cada segundo, para que sus sueños puedan estar al tanto en una app. La query tiene que devolver la ubicación más reciente de cada colla de cada dueño. Tablas: `CostumeLocations` y `CostumeOwners`.

Para escoger cuál es mejor optimizar, pienso que el primer problema es el mejor candidato, al tener más tablas relacionadas entre sí. Se puede hacer más separación de asusntos y luego recucir el peso del `JOIN` al consultar. Lo único que no calza es que la query solo se utiliza por los empleados cuando le dan refresh a los resultados.

Pero también puede resultar más necesario optimizar la query del tercer problema, dado que es un servicio más recurrente que le de los otros problemas y de índole crítica. Además, se puede optimizar un poco la relación de las dos tablas filtrandolas bien y luego uniendo.

Por lo tanto escogeré 3.

### 2) Make it easier to find Mitzie!

Busqueda de un perro que se perdió. Me dan una query hecha para lograr esto.

```sql
WITH LocationsAndOwners AS
(
   SELECT *
   FROM CostumeOwners co INNER JOIN CostumeLocations cl
      ON co.CostumeID = cl.CostumeID
),
LastSeen AS
(
   SELECT CostumeID, MAX(Timestamp)
   FROM LocationsAndOwners
   GROUP BY CostumeID
)
   SELECT lo.CostumeID, Location
   FROM LocationsAndOwners lo INNER JOIN LastSeen ls
      ON lo.Timestamp = ls.Timestamp AND lo.CostumeID = ls.CostumeID
   WHERE OwnerID = MitzieOwnerID
```

¿Hay alguna manera de hacerlo más barato y rápido?

En `LastSeen` no hace falta usar `LocationAndOwners` para sacar el `CostumeID` y `Timestamp`. Se puede hacer directamente con `CostumeLocations`, utilizando una tabla más pequeña y reduciendo el tiempo de búsqueda.

De hecho en `LocationAndOwners` ya se encuntran los datos necesarios para hacer la búsqueda(fijese que se una `SELECT *`). No hace falta hacer un `JOIN` con `LastSeen` para sacar la información necesaria. Se puede hacer directamente con `LocationAndOwners`.

Pero tiene un lado malo, que ese `SELECT *` es muy pesado para seguir trabajando con el en siguientes resultados. Tal vez se puede reducir el número de columnas que se seleccionan.


## Aprendizajes

### Exercises

1. Siempre es mejor optimizar la query que se usa más veces y tenga mayor volumen de resultados. Porque se ahorra más tiempo y recursos.
2. Primero minimizar las tablas a utitlizar al máximo antes de hacer operaciones aggregate o de analysis.

## References

Libro de SQL de O'Reilly. Para más información sobre optimización de querys.
[https://www.oreilly.com/library/view/google-bigquery-the/9781492044451/](https://www.oreilly.com/library/view/google-bigquery-the/9781492044451/)
