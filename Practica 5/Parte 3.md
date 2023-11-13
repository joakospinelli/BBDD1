# 6.

**CLIENTE** (<ins>id_cliente</ins>, nombreCliente, puntaje, edad)

**AUTOMOVIL** (<ins>id_automovil</ins>, marca, color)

**RESERVA** (<ins>id_cliente, id_automovil</ins>, fecha)

## a. Obtener los colores de los automóviles reservados por Juan.

RESERVAS_JUAN <= RESERVA |X| ( π <sub>idCliente</sub> ( σ <sub>nombreCliente = 'Juan'</sub> CLIENTE ) )

π <sub>color</sub> ( AUTOMOVIL |X| ( π <sub>id_automovil</sub> RESERVAS_JUAN ) )

## b. Obtener los nombres de los clientes que no han reservado un automóvil verde.

RESERVAS_VERDE <= RESERVA |X| ( π <sub>id_automovil</sub> ( σ <sub>color = 'Verde'</sub> AUTOMOVIL ) )

CLIENTES_VERDE <= CLIENTE |X| ( π <sub>id_cliente</sub> RESERVAS_VERDE )

π <sub>nombreCliente</sub> (CLIENTE - CLIENTES_VERDE)

# 7.

**PDA** (<ins>imei</ins>, marca, numero_serie)

**JURISDICCIÓN** (<ins>id_jurisdiccion</ins>, nombre)

**CONDUCTOR** (<ins>dni_conductor</ins>, nombre, apellido, id_Jurisdiccion)

**TIPO_INFRACCION** (<ins>codigo</ins>, descripcion, puntos, tipo)

**ACTA_INFRACCION** (<ins>#acta</ins>, imei, fecha, dni_conductor, id_Jurisdiccion)

**INFRACCION_ACTA** (<ins>#acta, codigo</ins>)

## a. Obtener los códigos de los tipos de infracciones que no fueron utilizadas en las actas labradas de la jurisdicción "La Plata".

ACTAS_LP <= ACTA_INFRACCIÓN |X| π <sub>idJurisdicción</sub> ( σ <sub>nombre = 'La Plata'</sub> JURISDICCIÓN )

TIPOS_LP <= TIPO_INFRACCIÓN |X| ( π <sub>codigo</sub> ( INFRACCIÓN_ACTA |X| π <sub>#acta</sub> ACTAS_LP) )

π <sub>código</sub> ( TIPO_INFRACCIÓN - TIPOS_LP )

## b. Obtener los #Actas en donde el conductor pertenezca a la misma jurisdicción del lugar del labrado del acta.

π <sub>#acta</sub> ( σ <sub>CONDUCTOR.id_Jurisdicción = ACTA_INFRACCIÓN.id_Jurisdicción</sub> ( ACTA_INFRACCIÓN X CONDUCTOR ) )

# 8.

**ALUMNO** (<ins>#alumno</ins>, nombre)

**CURSA** (<ins>#alumno, #curso</ins>)

**CURSO** (<ins>#curso</ins>, nombre_curso)

**PRACTICA** (<ins>#practica</ins>, #curso)

**ENTREGA** (<ins>#alumno, #practica</ins>, nota)

## a. Obtener #alumno y nombre de los alumnos que aprobaron con 7 o más todas las prácticas de los cursos que realizaron.

PRACTICAS_ALUMNO <= π <sub>#alumno, #práctica</sub> (CURSO |X| CURSA |X| PRÁCTICA)

ENTREGAS_NO_HECHAS <= PRACTICAS_ALUMNO - (π <sub>#alumno, #práctica</sub> ENTREGA)

ENTREGAS_DESAPROBADAS <= PRACTICAS_ALUMNO - ( π <sub>#alumno, #práctica</sub> (σ <sub>nota < 7</sub> ENTREGA ) )

π <sub>#alumno, #nombre</sub> ( ALUMNO |X| { π <sub>#alumno</sub> ALUMNO - [ π <sub>#alumno</sub> (ENTREGAS_NO_HECHAS U ENTREGAS_DESAPROBADAS ) ] } )