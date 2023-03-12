# Portfolio GBD - Marzo
#### Enrique García Navarro

## Resumen

A lo largo de este trimestre hemos aprendido a realizar consultas en **SQL**, primero de una forma básica listando solo columnas, luego hemos trabajado con funciones y por último hemos incorporado más de una tabla a las consultas, permitiendo así juntar dos o más tablas.
### Tema 6
En primer lugar tenemos que aprender la sintaxis básica de la instrucción `SELECT` para luego poder aplicar el resto de contenidos que aprendimos. Luego vimos una serie de funciones para trabajar con texto y con números, una vez terminado eso, añadimos a nuestro conocimiento las funciones para tratar nulos y evitar posibles inconvenientes que teníamos solo con las funciones de texto normales.

Luego tuvimos la necesidad de aprender a usar las funciones relacionadas con las fechas (aunque ya conociamos una de los insert: `to_date()`) así como las funciones relacionadas con la conversión de tipos (de numero a texto y viceversa por poner un ejemplo). Cada vez nos encontrabamos con consultas que requerían un tipo de funciones más avanzadas, concretamente las que usan expresiones regulares para así poder realizar consultas de forma más sencilla.

### Tema 7

En este tema añadimos a nuestro conocimiento el uso de una cláusula del `SELECT` que nos permite unir mediante los foreign keys dos o más tablas, esto hace posible unir el contenido de tal forma que una consulta que antes no podíamos realizar ahora pudieramos, concretamente el saber mediante un indice el nombre de algo, por ejemplo tenemos dos tablas en una de ellas están las provincias españolas y en otra tabla se encuentran las comunidades, pues en la tabla provincia hay una columna llamada id_comunidad, pongamos que es el 1, pero no sabemos que es ese 1, para ello recurrimos a la clausula `JOIN`, y así sabemos que el 1 es Madrid por ejemplo.

El uso de esta cláusula puede parecer sencillo, pero hay consultas, normalmente las recursivas, que no son tan sencillas de visualizar, o las consultas de producto cruzado para comparar filas.

En este tema, también hemos visto como hacer consultas de totales, es decir, poder sumar todos los valores de una columna, contar filas, obtener la media, o los mínimos y máximos; así como dos cláusulas necesarias para realizar estas operaciones, `HAVING` y `GROUP BY`.

## Reflexiones personales

A pesar de ser un mundo nuevo para mí, he tenido facilidad para aprender las funciones, como funcionan y como usarlas gracias a conocimientos previos en programación

## Ejercicios significativos


## Conclusiones

