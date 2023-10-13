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