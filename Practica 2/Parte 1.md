# 1. Indicar la opción correcta.

Dado el esquema:

*MapasPublicados(ID Mapa, proyección, escala Mapa, ID Sitio Web, dominio Sitio Web, especialidad Sitio Web, dueños Sitio Web, fecha Publicación Mapa, valor Publicación)*

* A un sitio web se le cobra un valor (`valorPublicación`) por cada fecha
(`fechaPublicaciónMapa`) en la cual publique un mapa.
* Un sitio web puede tener varios dueños (`dueñosSitioWeb`).
* Un sitio web posee un único dominio (`dominioSitioWeb`).
* El identificador de un mapa (`idMapa`) es único.
* El identificador de un sitio web (`idSitioWeb`) es único.
* Un mapa se genera con una proyección y a una escala.
* `especialidadSitioWeb` es la especialidad de un sitio.

DF:
1. ID Mapa -> proyección, escala
2. ID Sitio Web -> dominio, especialidad
3. ID Mapa, ID Sitio Web, fecha Publicación -> valor publicación
4. Dominio Sitio Web -> ID Sitio Web, especialidad

***// No puedo determinar los dueños del sitio a partir de ID/Dominio porque puede haber más de uno***

El esquema tiene más de una clave candidata; estas son:
* `{ ID Mapa, ID Sitio Web, fecha Publicación Mapa, dueños Sitio web }`
* `{ ID Mapa, dominio Sitio Web, fecha Publicación Mapa, dueños Sitio web }`

# 2. Clave candidata

Encontrar las posibles claves candidatas del siguiente esquema:

*E(a, b, c, d, e, f)*

DF:
1. a -> b, c
2. c -> d, e

La clave candidata sería `{ a, f }`, puesto que es el único conjunto a partir del cual se pueden determinar todos los demás atributos:
* `a` determina a `b` y `c`.
    * `c` determina a `d` y `e`
* `f` no está determinado por ningún otro atributo, por lo que debe incluirse en la clave.

# 3. Indicar la opción correcta

Dada la relación:

*Alumno (DNI, nombre-apellido, nro. legajo, promedio, #libro usado en carrera)*

En la que se cumple las siguientes dependencias funcionales:

1. DNI → nombre-apellido, nro. legajo, promedio
2. nro. legajo → nombre-apellido, DNI,promedio

La relación `Alumno` tiene dos claves candidatas y tendrá una clave primaria.

Las posibles CC serían `DNI` y `nroLegajo`, puesto que a partir de ellas se pueden determinar todos los demás atributos. Sin embargo, el diseño de la BD siempre restringe la clave primaria a una sola, aunque haya más de una candidata.

***// Se tendría que incluir #libro en la CC? No se puede determinar con las DF***

# 4. Dependencias funcionales

Dado el siguiente esquema:

*Tienda (#aplicación, nombre aplicación, descripción, #categoría, #etiqueta, #desarrollador, nombre-apellido desarrollador, #actualización, descripción cambios)*
* `#aplicación`, `#categoría`, `#etiqueta` y `#desarrollador` son únicos en el sistema.
* Una aplicación tiene un nombre y una descripción, y puede actualizarse muchas
veces
* Para cada actualización de una aplicación se registra un texto con los cambios realizados. El `#actualización` es secuencial, cada aplicación define los suyos y puede repetirse entre distintas aplicaciones.
* Cada aplicación tiene una única categoría y muchas etiquetas. Las etiquetas
pueden ir cambiando con cada actualización de la aplicación (en cada
actualización puede haber un conjunto diferente de etiquetas). La categoría
nunca cambia, es decir que se mantiene igual sin importar las actualizaciones.
* Una aplicación es realizada por varios desarrolladores de los cuales se conoce su nombre y apellido.

Claves candidatas:
* `{ #aplicación, #actualización, #desarrollador }`

***// Debería agregar `#etiqueta` a la CC? No se puede determinar a partir de una DF***

Indicar las DF válidas:
1. #aplicación, #actualización -> nombre aplicación, descripción ✅
2. #aplicación, #actualización -> descripción cambios ✅
3. nombre-apellido desarrollador -> #desarrollador ❌
4. #desarrollador -> nombre-apellido desarrollador ✅
5. #aplicación -> #categoria ✅

La 3 no es válida porque no se puede determinar un atributo único a partir de uno repetible (por ejemplo, si hay varios desarrolladores con el mismo nombre).

# 5. Dependencias multivaluadas

Dado el siguiente esquema:

*CURSOS(#curso, título curso, #módulo, titulo módulo, contenido módulo, nombre autor, email autor, contraseña autor, año edición, calificación, referencia)*

* Cada curso (`#curso`) se va editando todos los años, y en cada año (`año edición`) puede cambiar sus módulos, no así el título y el autor.
* En cada año que se edita un curso, recibe varias calificaciones anónimas.
* El email de cada autor se usa como Login, y no puede repetirse en el sistema.
* Los números de módulo (`#módulo`) son secuenciales (módulo 1, 2, 3, etc).
Es decir, en cada edición de cada curso se enumeran los módulos de la misma
forma, y se pueden repetir en diferentes ediciones de cursos.
* Cada curso tiene múltiples referencias bibliográficas, que se mantienen a través de todas sus ediciones.

Claves candidatas:
* `{ #curso, año edición, #módulo, calificación, referencia }`

Y dada la relación en BCFN: *CURSOS_N (<ins>#curso, año edición, #módulo, calificacion, referencia</ins>)*

Seleccione las DM que son válidas a la vez en el esquema *CURSOS_N*:
* #curso ->> año edición ✅
* #curso ->> referencia ✅
* #curso, año edición ->> calificación ✅
* referencia ->> #curso ❌
    * ***// Podría haber una misma ref en distintos cursos? además hay varias ref***
* año edición ->> #curso ❌
