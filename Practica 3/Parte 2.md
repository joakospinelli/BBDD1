# 6.

DUEÑO (<ins>id_dueño</ins>, nombre, teléfono, dirección, dni)

CHOFER (<ins>id_chofer</ins>, nombre, teléfono, dirección, fecha_licencia_desde, fecha_licencia_hasta, dni)

AUTO (<ins>patente</ins>, id_dueño, id_chofer, marca, modelo, año)

VIAJE (<ins>patente</ins>, hora_desde, hora_hasta, origen, destino, tarifa, metraje)

## a. Listar el DNI, nombre y teléfono de todos los dueños que NO son choferes

DUEÑOS_NO_CHOFERES <- π <sub>dni</sub> (DUEÑO) - π <sub>dni</sub> (CHOFER)

π <sub>dni, nombre, teléfono</sub> (DUEÑO |X| DUEÑOS_NO_CHOFERES)

## b. Listar la patente y el id_chofer de todos los autos a cuyos choferes les caduca la licencia el 01/01/2024

CHOFERES_CADUCACION_ENERO <- σ <sub>fecha_licencia_hasta = 01/01/2023</sub> (CHOFER)

AUTOS_CADUCACION_ENERO <- AUTO |X| π <sub>id_chofer</sub> (CHOFERES_CADUCACION_ENERO)

π <sub>patente, id_chofer</sub> (AUTOS_CADUCACION_ENERO)

# 7.

ESTUDIANTE (<ins>#legajo</ins>, nombreCompleto, nacionalidad, añoDeIngreso, códigoDeCarrera)

CARRERA (<ins>códigoDeCarrera</ins>, nombre)

INSCRIPCIONAMATERIA (<ins>#legajo, códigoDeMateria</ins>)

MATERIA (<ins>códigoDeMateria</ins>, nombre)

## a. Obtener el nombre de los estudiantes que ingresaron en 2019.

π <sub>nombreCompleto</sub> (σ <sub>añoDeIngreso = 2019</sub> (ESTUDIANTE) )

## b. Obtener el nombre de los estudiantes con nacionalidad "Argentina" que NO estén en la carrera con código "LI07"

ESTUDIANTES_LI07 <- σ <sub>códigoDeCarrera = "LI07"</sub> (ESTUDIANTE)

ESTUDIANTES_ARGENTINOS <- σ <sub>nacionalidad = "Argentina"</sub> (ESTUDIANTE)

π <sub>nombreCompleto</sub> (ESTUDIANTES_ARGENTINOS - ESTUDIANTES_LI07)

## c. Obtener el legajo de los estudiantes que se hayan anotado en TODAS las materias.

INSCRIPCIONES <- INSCRIPCIONAMATERIA % π <sub>códigoDeMateria</sub> (MATERIA)

π <sub>#legajo</sub> (INSCRIPCIONES)

*// creo que la división ya me devuelve sólo #legajo así que no tendría que hacer la proyección al final*

# 8.

LUGAR_TRABAJO (<ins>#empleado, #departamento</ins>)

CURSO_EXIGIDO (<ins>#departamento, #curso</ins>)

CURSO_REALIZADO (<ins>#empleado, #curso</ins>)

## a. ¿Quiénes son los empleados que han hecho todos los cursos, independientemente de qué departamento los exija?

CURSOS <- π <sub>#curso</sub> (CURSO_EXIGIDO)

π <sub>#empleado</sub> (CURSO_REALIZADO % CURSOS)

## b. ¿Quiénes son los empleados que ya han realizado todos los cursos exigidos por sus departamentos?

CURSOS_EMPLEADO <- LUGAR_TRABAJO |X| CURSO_REALIZADO *// genera esquema `(#empleado, #depto, #curso)`*

π <sub>#empleado</sub> (CURSO_EMPLEADO % CURSO_EXIGIDO)

# 9.

FRECUENTA (<ins>bebedor, bar</ins>)
SIRVE (<ins>bar, cerveza</ins>)
GUSTA (<ins>bebedor, cerveza</ins>)
## a. ¿Cuáles son los bares que sirven cerveza que le gusta al bebedor `x`?

CERVEZAS_X <- π <sub>cerveza</sub> (σ <sub>bebedor = "x"</sub> (GUSTA))

π <sub>bar</sub> (SIRVE |X| CERVEZAS_X)
## b. ¿Quiénes son los bebedores que frecuentan al menos un bar que sirve una cerveza que les gusta?

π <sub>bebedor</sub> (FRECUENTA |X| SIRVE |X| GUSTA)
## c. ¿Quiénes son los bebedores que frecuentan sólo bares que sirven solo las cervezas que les gustan? (cada bebedor gusta de al menos una cerveza y frecuenta al menos un bar)

FRECUENTA_GUSTA <- FRECUENTA |X| GUSTA *// genera esquema `(bebedor, bar, cerveza)`*

FRECUENTA_GUSTA % SIRVE

## d. ¿Quiénes son los bebedores que no frecuentan ningún bar que sirve alguna cerveza que les gusta?

BAR_SIRVE_GUSTA <- π <sub>bebedor, bar</sub> (GUSTA |X| SIRVE)

π <sub>bebedor</sub> (BAR_SIRVE_GUSTA - FRECUENTA)
## e. ¿Quiénes son los bebedores que gustan de todas las cervezas que sirve el bar `y` ?

CERVEZAS_Y <- σ <sub>bar = "y"</sub> (SIRVE)

GUSTA % π <sub>cerveza</sub> (CERVEZAS_Y)

## f. ¿Quiénes son los bebedores que gustan de todas las cervezas que sirven en todos los bares?

CERVEZAS <- π <sub>cerveza</sub> (SIRVE)

CERVEZAS_TODOS <- SIRVE % CERVEZAS

GUSTA % π <sub>cerveza</sub> CERVEZAS_TODOS