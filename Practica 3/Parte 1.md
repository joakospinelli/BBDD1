# 1.

## a. Indique cuáles de las siguientes operaciones son válidas:
* `A(a,b,c) U B(a,b,d)`: ❌ A y B no son unión-compatibles puesto que los atributos `c` y `d` no pertenecen al mismo dominio.
* `(A(a,b,c) |X| B(a,b)) - C(a,b,c)`: ✅ El producto natural entre A y B genera una relación `(a,b,c)`, que es unión-compatible con C.
* `(A(a,b,c) |X| B(a,d,e)) ∩ D(a,b,c,d,e)` ✅ El producto natural entre A y B genera una relación `(a,b,c,d,e)`, que es unión compatible con D.
* `(A(a,b,c) X B (a,b,d)) ∩ D(a,b,c,d)`: ❌ El producto cartesiano no elimina atributos con nombre duplicados, por lo que genera una relación que no es unión compatible con D.

## b. Para la operación de resta es necesario que los esquemas involucrados sean compatibles, es decir, deben cumplir las siguientes condiciones:
* Deben tener la misma cantidad de columnas ✅
* Las columnas deben ser del mismo dominio ✅
* El orden de los columnas debe ser el mismo ✅
* Las columnas deben tener igual nombre ❌

# 2. ¿Para cuáles de las siguientes operaciones es necesario que los operandos sean union compatibles? Marque todas las opciones correctas:
* Resta `-`: ✅
* División `%`: ❌
* Unión `U`: ✅
* Producto cartesiano `X`: ❌
* Producto natural `|X|`: ❌

# 3. Dados los siguientes esquemas indique qué formato (conjunto de atributos) tiene el resultado de aplicar la siguiente operación.

*COMPRA(#compra, fecha, monto_total)*

*COMPRA_PRODUCTO(#compra, cantidad, #producto)*

*PRODUCTO(#producto, nombre, precio)*


COMPRA_PRODUCTO % π <sub>#producto</sub> (PRODUCTO)

* (#compra, cantidad) ✅
* (#compra, cantidad, #producto) ❌
* (#compra) ❌

El resultado final de la división elimina los atributos del divisor; en este caso, `#producto`.

# 4. Dado el siguiente esquema indicar si las siguientes consultas obtienen el resultado correcto (sin importar la optimización).

PASAJERO (<ins>#pasajero</ins>, nombre, dni, puntaje)

PASAJERO_RESERVA (</ins>#pasajero, #reserva</ins>)

RESERVA (</ins>#reserva</ins>, #vuelo, fecha_reserva, monto, #asiento)

VUELO (<ins>#vuelo</ins>, aeropuerto_salida, aeropuerto_destino, fecha_vuelo)

## a. Obtener los pasajeros que tengan reservas sobre vuelos del próximo año, listando `#pasajero`, `#vuelo` y `#asiento`.

VUELOS_PROX_AÑO <— σ <sub>fecha_vuelo >= 1/1/2024 AND fecha_vuelo <= 31/12/2024</sub> (VUELO)

RES <— π <sub>#pasajero,#vuelo,#asiento</sub> (VUELOS_PROX_AÑO |X| RESERVA |X| PASAJERO_RESERVA)

1. `VUELOS_PROX_AÑO` obtiene todos los vuelos reservados para el próximo año.
2. `RES` realiza dos productos naturales y una proyección:
    * Primero obtiene la relación entre el vuelo y la reserva.
    * Luego obtiene a los pasajeros que realizaron las reservas.
    * Por último, proyecta los campos `#pasajero`, `#vuelo` y `#asiento`.

✅ La consulta es correcta.

## b. Obtener el listado de montos de reservas realizadas para vuelos efectuados el pasado Agosto desde Buenos Aires a Córdoba.

VUELOS_BUE_CBA <— σ <sub>ciudad_salida="Buenos Aires" AND ciudad_destino="Córdoba"</sub> (VUELO)

RESERV_AGO <— (σ <sub>fecha_reserva >= 1/8/2023 AND fecha_reserva <= 31/8/2023</sub> (RESERVA)) |X| VUELOS_BUE_CBA

RES <— π <sub>monto_total</sub> RESERV_AGO

❌ La consulta es incorrecta. Al realizar la consulta `VUELOS_BUE_CBA` se usan los campos "*ciudad_salida*" y "*ciudad_destino*" que no existen en el esquema.

## c. Obtener el/los pasajeros con reservas en vuelos pendientes que tengan menor puntaje

PUN_ALTOS <— σ <sub>pun < puntaje</sub> (PASAJERO X (ρ <sub>#p, nom, d, pun</sub> (PASAJERO) ))

PUN_BAJOS <— ( π <sub>puntaje</sub> (PASAJEROS) ) - ( π <sub>puntaje</sub> PUN_ALTOS)

RES <— PASAJEROS |X| PUN_BAJOS

*// Vamos a ignorar que a veces dice PASAJERO y otras dice PASAJEROS*

1. `PUN_ALTOS` realiza un producto entre pasajeros para poder obtener aquellas tuplas en las que un pasajero tiene más puntos que otro.
2. `PUN_BAJOS` realiza la diferencia entre todos los puntajes y los puntajes altos obtenidos en `PUN_ALTOS`, para obtener los puntajes bajos.
3. `RES` obtiene los pasajeros con dichos puntajes bajos.

❌ La consulta es incorrecta. Obtiene los pasajeros con menor puntaje, pero nunca verifica que tengan vuelos pendientes.

## d.  Obtener el/los pasajeros que solo hayan reservado vuelos cuyo aeropuerto de salida sea el aeropuerto “Ministro Pistarini”. Listar el nombre y DNI de los pasajeros.

VUELOS_PISTARINI <— π <sub>#vuelo</sub> (σ <sub>aeropuerto_salida="Ministro Pistarini"</sub> (VUELO))

RESERVA_PISTARINI <— π <sub>#pasajero</sub> (VUELOS_PISTARINI |X| RESERVAS)

PASAJEROS_PISTARINI <— π <sub>nombre,dni</sub> (RESERVA_PISTARINI |X| PASAJERO)

1. `VUELOS_PISTARINI` obtiene los vuelos que su aeropuerto de salida sea "*Ministro Pistarini"* y proyecta sólo los números de los vuelso.
2. `RESERVA_PISTARINI` realiza un producto natural para obtener las reservas a vuelos con dicho aeropuerto de salida, y proyecta los números de pasajeros.

❌ La consulta es incorrecta. En la consulta `RESERVA_PISTARINI` intenta proyecta el número de pasajero, pero ese atributo no se encuentra en el esquema resultante del producto natural.

## e. Obtener los ID de los pasajeros que hayan realizado reservas por un monto superior a $99000

RESERVAS_MAS_99000 <— π <sub>#pasajero</sub> (σ <sub>monto < 99000</sub> (RESERVAS))

❌ La consulta es incorrecta. Hay dos errores:
* `#pasajero` no se encuentra en la relación *RESERVAS*, por lo que no se puede proyectar.
* La condición de la selección debería ser `monto > 99000`.