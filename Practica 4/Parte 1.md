# 1.

DUEÑO (<ins>id_dueño</ins>, nombre, teléfono, dirección, dni)

CHOFER (<ins>id_chofer</ins>, nombre, teléfono, dirección, fecha_licencia_desde, fecha_licencia_hasta, dni)

AUTO (<ins>patente</ins>, id_dueño, id_chofer, marca, modelo, año)

VIAJE (<ins>patente, hora_desde</ins>, hora_hasta, origen, destino, tarifa, metraje)

## a. Listar el DNI, nombre y teléfono de todos los dueños que NO son choferes

```sql
SELECT d.dni, d.nombre, d.telefono
FROM Dueño AS d
WHERE d.dni NOT IN (
    SELECT dni
    FROM Chofer
)
```
## b. Listar la patente y el id_chofer de todos los autos a cuyos choferes les caduca la licencia el 01/01/2024

```sql
SELECT DISTINCT a.patente, c.id_chofer
FROM Chofer AS c INNER JOIN Auto AS a ON c.id_chofer = a.id_chofer
WHERE c.fecha_licencia_hasta = 01/01/2024
```

# 2.

ESTUDIANTE (<ins>#legajo</ins>, nombreCompleto, nacionalidad, añoDeIngreso, códigoDeCarrera)

CARRERA (<ins>códigoDeCarrera</ins>, nombre)

INSCRIPCIONAMATERIA (<ins>#legajo, códigoDeMateria</ins>)

MATERIA (<ins>códigoDeMateria</ins>, nombre)

## a. Obtener el nombre de los estudiantes que ingresaron en 2019.

```sql
SELECT nombreCompleto
FROM Estudiante
WHERE añoDeIngreso BETWEEN 01/01/2019 AND 31/12/2019
```

## b. Obtener el nombre de los estudiantes con nacionalidad "Argentina" que NO estén en la carrera con código "LI07"

```sql
SELECT e.nombreCompleto
FROM Estudiante AS e
WHERE NOT EXISTS (
    SELECT *
    FROM InscripcionAMateria AS im
    WHERE e.legajo = im.legajo AND im.códigoDeMateria = "LI07"
)
```

# Compare las resoluciones de estos ejercicios con las realizadas en álgebra relacional. ¿Qué paralelismo encuentra entre las diferentes operaciones de AR en SQL y en las formas de las resoluciones?

Las operaciones de SQL están basadas en el álgebra relacional, por lo que se encuentran paralelismos bastante evidentes entre ambos lenguajes:
* `SELECT <...atributos>` equivale a π <sub>...atributos</sub>
* `WHERE <condición>` equivale a σ <sub>condición</sub>
* `<tabla> AS <nombre_nuevo>` equivale a ρ <sub>nombre_nuevo</sub> Tabla
* `INNER JOIN <condición>` es un producto natural, con la diferencia de que permite que el desarrollador eliga la condición por la que se unen ambas tablas.
