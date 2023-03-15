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

A pesar de ser un mundo nuevo para mí, he tenido facilidad para aprender las funciones, como funcionan y como usarlas gracias a conocimientos previos de programación. Sí bien es cierto, me he complicado en ocasiones para resolver ciertos ejercicios, cuando con una línea se podía (ejercicio de los años bisiestos), y en otras ocasiones lo que me ha costado ha sido el sacar las relaciones entre tablas, para saber si tengo que usar un `join on` o uno de tipo `outer join`, pero a parte de eso, no he tenido mayores complicaciones.

Decidir lo que es más positivo y menos me resulta complicado, ya que todo me parece importante, mas puedo decir que si no sabemos hacer consultas básicas no podremos hacer mucho más, y luego si no sabemos usar funciones o juntar tablas, estaremos muy limitados para realizar consultas complejas.

## Ejercicios significativos

De todas las prácticas que hemos hecho me resulta fácil decir cuales son los ejercicios que más me han costado o me han parecido interesante. Comenzaré desde la primera práctica de SELECT (la 14 de todas ellas).
```sql
SELECT NOMBRE,
       APELLIDO1,
       APELLIDO2,
       CASE
           WHEN UPPER(TRIM(SUBSTR(DIRECCION, 1, INSTR(DIRECCION, ' ')))) = 'C'
               THEN 'Calle' -- ponemos el upper porque hay calles con c minuscula y otras con máyusculas
           ELSE INITCAP(TRIM(SUBSTR(DIRECCION, 1, INSTR(DIRECCION, ' ')))) -- Ya de paso pongo la primera letra en mayúsculas
           END
                                                                                              AS TIPO_VIA,
       TRIM(SUBSTR(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))), 1,
                   INSTR(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))), ' ', 1,
                         REGEXP_COUNT(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))), ' ')))) AS Nombre_VIA,
       TRIM(SUBSTR(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))),
                   INSTR(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))), ' ', 1,
                         REGEXP_COUNT(TRIM(SUBSTR(DIRECCION, INSTR(DIRECCION, ' '))), ' ')))) AS NUMERO_VIA
FROM ALUMNOS;
```
En esta consulta se nos daba una tabla que contenía una dirección, y se nos pedía separar en 3 columnas los tipos de vía (si son calles, avenidas, caminos...), el nombre de la vía y el número. Para nosotros, mentalmente nos parece fácil de hacer, pero claro, el transformarlo a código es algo más complicado, estuve bastante rato intentandolo hasta que salío ese "trozo" de código.

Luego se fue complicando todo un poco, pero llegaron las tablas múltiples y aquí si tuve algún que otro problema.
```sql
SELECT P.NOMBRE, PADRE.NOMBRE, MADRE.NOMBRE
FROM PERSONAS P
         LEFT OUTER JOIN PERSONAS PADRE ON P.N_PADRE = PADRE.N_PERSONA
         LEFT OUTER JOIN PERSONAS MADRE ON P.N_MADRE = MADRE.N_PERSONA;
```
Este ejercicio consistia en sacar a todos las personas con sus padres, incluidas aquellas que no tenían (en la base de datos) los padres y madres, que puede parecer fácil pero no se porque en mi cabeza tenía que usar `right outer join` cosa que no es necesario y me estaba peleando porque no salía.

```sql
SELECT p.nombre
FROM personas p
         RIGHT OUTER JOIN personas pa ON (p.n_padre = pa.n_persona)
WHERE pa.sexo = 'H'
  AND p.nombre is null;
```
En este caso se nos pide obtener las personas que no son padres.
```sql
SELECT descripcion, minuto, j.NOMBRE
FROM goles g
         NATURAL JOIN jugadores j
         cross JOIN jugadores J2
where G.DESCRIPCION like '%' || J2.NOMBRE || '%';
```
En este ejercicio tienes que usar un `cross join` para hacer una consulta para comparar los jugadores de futbol y la descripción de los goles de tal forma que observamos que el gol ha sido asistido por otros jugadores. Otra cosa que he usado ha sido un `natural join` ya que coincidia el nombre de las columnas de ambas tablas que las relacionaban.

```sql
SELECT P.NOMBRE, COUNT(*) MUNICIPIOS
FROM LOCALIDADES L
JOIN PROVINCIAS P on L.N_PROVINCIA = P.N_PROVINCIA
GROUP BY P.NOMBRE
HAVING COUNT(*) = (SELECT MAX(COUNT(*)) maximo
                   FROM localidades l
                   JOIN provincias p USING (n_provincia)
                   GROUP BY p.nombre);
```
Y esta es una de las más complejas en el sentido de mostrar el máximo de municipios y otra columna que sea el nombre de la provincia, ya que no se puede hacer en una sola consulta y no queda más remedio que usar una subconsulta.

## Ejercicios de Invención propia
Un ejercicio que se me ocurre es una consulta que me salió intentando hacer otra cosa.
Obtener la captial de la comunidad cuyo nombre es diferente al de la provincia y la provincia es diferente a la propia comunidad.
```sql
SELECT L.NOMBRE,P.NOMBRE, C.NOMBRE
FROM LOCALIDADES L
    JOIN COMUNIDADES C ON L.ID_LOCALIDAD = C.ID_CAPITAL
    JOIN PROVINCIAS P on L.N_PROVINCIA = P.N_PROVINCIA
WHERE L.NOMBRE != P.NOMBRE AND P.NOMBRE != C.NOMBRE;
```
Explicación: Leemos desde la tabla localidades, luego unimos comunidades a traves de la relación C.ID_CAPITAL y L.ID_LOCALIDAD y luego unimos provincias mediante la foreign key de N_PROVINCIA.

Otro ejercicio es uno que he puesto antes, obtener el nombre de la provincia y el numero de localidades que más tiene. La unica diferencia es que se nos pedia solo el número y yo saqué tambien el nombré de la provincia.

```sql
SELECT P.NOMBRE, COUNT(*) MUNICIPIOS
FROM LOCALIDADES L
JOIN PROVINCIAS P on L.N_PROVINCIA = P.N_PROVINCIA
GROUP BY P.NOMBRE
HAVING COUNT(*) = (SELECT MAX(COUNT(*)) maximo
                   FROM localidades l
                   JOIN provincias p USING (n_provincia)
                   GROUP BY p.nombre);
```

## Conclusiones

A lo largo de este trimestre sin duda alguna todo ha sido nuevo para mí, ya que nunca había trabajado con **SQL**, pero muy gratificante el aprender el como se obtiene la información de una base de datos, como manipularla para obtener solo lo que necesitamos o el consultar varias tablas para obtener información más completa.
