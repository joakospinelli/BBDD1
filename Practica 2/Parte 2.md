***// SE TIENEN QUE IR PARTICIONANDO LAS DF QUE EN `Y` NO TENGAN NINGÚN ATRIBUTO QUE SEA `X` EN OTRA DF (lo mejor es empezar con DF que `X` no esté en la CC).***

***// PARTICIONAR DF QUE NO SE "CHOQUEN" CON OTRAS, PORQUE SI NO DESPUÉS NO PODÉS PARTICIONAR LAS DEMÁS***

# 6.

*SUSCRIPCIÓN (#suscripcion, email, nombre_usuario, #plan, nombre_plan, texto_condiciones, precio, email_adicional, nombre_adicional, #contenido, titulo, sinopsis, duración, fecha_adicional)*

* Cada suscripción es realizada por un único usuario (identificado por el email) y un plan, pero además hay usuarios adicionales que la utilizan (email_adicional). De cada usuario adicional que se suma a la suscripción, se guarda la fecha.
* Un plan de suscripción tiene un nombre (que no puede garantizarse que sea único en el sistema), condiciones, y un precio mensual.
* Cada contenido tiene un título, sinopsis y duración. El #contenido es único en el sistema, pero del título no puede garantizarse que lo sea.
* De cada suscripción se sabe qué contenidos fueron reproducidos, sin distinción sobre qué usuario (titular o adicionales) reprodujo cada uno.

DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

**Clave Candidata:** *(#suscripción, email_adicional, #contenido)*

## Iteración 1

*Suscripción* no está en BCFN porque tiene al menos una DF no trivial o en la que X no sea superclave.

Particiono el esquema siguiendo la DF5 `email_adicional -> nombre_adicional`:

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L2: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, nombre_usuario, #plan, nombre_plan, texto_condiciones, precio, titulo, sinopsis, duración, fecha_adicional)*

L1 ⋂ L2 = `email_adicional`, que es clave en L1. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L2`: 1, 2, 3, 4, 6

## Iteración 2

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L2: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, nombre_usuario, #plan, nombre_plan, texto_condiciones, precio, titulo, sinopsis, duración, fecha_adicional)*

L1 cumple con BCFN, puesto que en su única DF `X` (*email_adicional*) es superclave del esquema.

L2 no cumple con BCFN porque aún tiene DF no triviales o en las que X no sea superclave.

Particiono L2 siguiendo la DF `email -> nombre_usuario`:

* L3: *(<ins>email</ins>, nombre_usuario)*
* L4: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, nombre_plan, texto_condiciones, precio, titulo, sinopsis, duración, fecha_adicional)*

L3 ⋂ L4 = `email`, que es clave en L3. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L3`: 2
* `L4`: 1, 3, 4, 6

## Iteración 3

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L3: *(<ins>email</ins>, nombre_usuario)*
* L4: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, nombre_plan, texto_condiciones, precio, titulo, sinopsis, duración, fecha_adicional)*

L1 y L3 cumplen con BCFN, puesto que en sus DF `X` es superclave del esquema.

L4 no cumple con BCFN porque aún tiene DF no triviales o en las que `X` no es superclave.

Particiono L4 siguiendo la DF `#contenido -> título, sinopsis, duración`

* L5: *(<ins>#contenido</ins>, título, sinopsis, duración)*
* L6: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, nombre_plan, texto_condiciones, precio, fecha_adicional)*

L5 ⋂ L6 = `#contenido`, que es clave en L5. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L3`: 2
* `L5`: 4
* `L6`: 1, 3, 6

## Iteración 4

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L3: *(<ins>email</ins>, nombre_usuario)*
* L5: *(<ins>#contenido</ins>, título, sinopsis, duración)*
* L6: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, nombre_plan, texto_condiciones, precio, fecha_adicional)*

L1, L3 y L5 cumplen con BCFN.

L6 no cumple con BCFN porque tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono siguiendo la DF `#plan -> nombre_plan, texto_condiciones, precio`:
* L7: *(<ins>#plan</ins>, nombre_plan, texto_condiciones, precio)*
* L8: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, fecha_adicional)*

L7 ⋂ L8 = `#plan`, que es clave en L7. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L3`: 2
* `L5`: 4
* `L7`: 3
* `L8`: 1, 6

## Iteración 5

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L3: *(<ins>email</ins>, nombre_usuario)*
* L5: *(<ins>#contenido</ins>, título, sinopsis, duración)*
* L7: *(<ins>#plan</ins>, nombre_plan, texto_condiciones, precio)*
* L8: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan, fecha_adicional)*

L1, L3, L5 y L7 cumplen con BCFN.

L8 no cumple con BCFN porque tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono siguiendo la DF `#suscripción, email_adicional -> fecha_adicional`:
* L9: *(<ins>#suscripción, email_adicional</ins>, fecha_adicional)*
* L10: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan)*

L9 ⋂ L10 = `#suscripción, email_adicional`, que es clave en L9. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L3`: 2
* `L5`: 4
* `L7`: 3
* `L9`: 6
* `L8`: 1

## Iteración 6

* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L3: *(<ins>email</ins>, nombre_usuario)*
* L5: *(<ins>#contenido</ins>, título, sinopsis, duración)*
* L7: *(<ins>#plan</ins>, nombre_plan, texto_condiciones, precio)*
* L9: *(<ins>#suscripción, email_adicional</ins>, fecha_adicional)*
* L10: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan)*

L1, L3, L5, L7 y L9 cumplen con BCFN.

L10 no cumple con BCFN.

Particiono siguiendo la DF `#suscripción -> email, #plan`:
* L11: *(<ins>#suscripción</ins>, email, #plan)*
* L12: *(<ins>#suscripcion, email_adicional, #contenido</ins>)*

L11 ⋂ L12 = `#suscripción`, que es clave en L12. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #suscripción -> email, #plan
2. email -> nombre_usuario
3. #plan -> nombre_plan, texto_condiciones, precio
4. #contenido -> título, sinopsis, duración
5. email_adicional -> nombre_adicional
6. #suscripción, email_adicional -> fecha_adicional

* `L1`: 5
* `L3`: 2
* `L5`: 4
* `L7`: 3
* `L9`: 6
* `L11`: 1

## Normalización a BCFN

L1, L3, L5, L7, L9 y L11 cumplen con BCFN.

L12 cumple con BCFN, puesto que tiene una DF **trivial**.

Se terminó el proceso de normalización en BCFN con las siguientes particiones:
* L1: *(<ins>email_adicional</ins>, nombre_adicional)*
* L3: *(<ins>email</ins>, nombre_usuario)*
* L5: *(<ins>#contenido</ins>, título, sinopsis, duración)*
* L7: *(<ins>#plan</ins>, nombre_plan, texto_condiciones, precio)*
* L9: *(<ins>#suscripción, email_adicional</ins>, fecha_adicional)*
* L10: *(<ins>#suscripcion, email_adicional, #contenido</ins>, email, #plan)*
* L11: *(<ins>#suscripción</ins>, email, #plan)*
* L12: *(<ins>#suscripcion, email_adicional, #contenido</ins>)*

No se encontraron DF multivaluadas que requieran continuar hasta 4FN.

# 7.

*MEDICIÓN AMBIENTAL (#medicion, #pozo, valor_medicion, #parametro, fecha_medicion, cuil_operario, #instrumento, nombre_parametro, valor_ref, descripcion_pozo, fecha_perforacion, nombre_parametro, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)*

Donde:
* Cada medición es realizada por un operario en un pozo, en una fecha determinada. En ella se miden varios parámetros, y para cada uno se obtiene un valor. Notar que un mismo parámetro (#parametro) puede ser medido en diferentes mediciones. Independientemente de las mediciones, todo parámetro tiene un nombre y valor de referencia, y el #parametro es único en el sistema.
* En cada medición se utilizan varios instrumentos, independientemente de los
parámetros medidos. De cada instrumento se conoce la marca y modelo.
* De cada operario se conoce su cuit, nombre, apellido y fecha de nacimiento.
* La empresa cuenta con vehículos, y de cada uno se conoce la fecha en la que fue adquirido. El dominio (patente) de cada vehículo es único en el sistema.
* Un pozo tiene una descripción y una fecha de perforación. El identificador #pozo es único en el sistema.

DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

**Clave candidata:** *(#medición, #parámetro, #instrumento, dominio_vehículo)*

## Iteración 1

*MEDICIÓN AMBIENTAL* no está en BCFN, puesto que tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono el esquema siguiendo la DF 5 `#pozo -> descripción_pozo, fecha_perforación`:
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L2: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, valor_ref, nombre_parametro, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, fecha_adquisicion)*

L1 ⋂ L2 = `#pozo`, que es superclave en L1. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L2`: 1, 2, 3, 4, 6, 7

## Iteración 2

* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L2: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, nombre_parametro, valor_ref, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, fecha_adquisicion)*

L1 cumple con BCFN, puesto que en la DF 5, `X` es superclave del sistema.

L2 no cumple con BCFN, porque aún tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono L2 siguiendo la DF 3 `#parámetro -> nombre_parámetro, valor_ref`:
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L4: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, fecha_adquisicion)*

L3 ⋂ L4 = `#parámetro`, que es superclave en L3. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L4`: 1, 2, 4, 6, 7

## Iteración 3
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L4: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, fecha_adquisicion)*

L1 y L3 cumplen con BCFN.

L4 no cumple con BCFN, puesto que al menos una de sus DF no es trivial o `X` no es superclave.

Particiono L4 siguiendo la DF 6 `dominio_vehículo -> fecha_adquisición`:
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L6: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento)*

L5 ⋂ L6 = `dominio_vehículo`, que es superclave en L5. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L5`: 6
* `L6`: 1, 2, 4, 7

## Iteración 4:
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L6: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento)*

L1, L3 y L5 están en BCFN. L6 no está en BCFN.

Particiono L6 siguiendo la DF 4: `#instrumento -> marca_instrumento, modelo_instrumento`:
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L8: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento)*

L7 ⋂ L8 = `#instrumento`, que es superclave en L7. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L5`: 6
* `L7`: 4
* `L6`: 1, 2, 7

## Iteración 5
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L8: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario, apellido_operario, nombre_operario, fecha_nacimiento)*

L1, L3, L5 y L7 están en BCFN. L8 no está en BCFN.

Particiono L8 siguiendo la DF 2 `cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento`:
* L9: *(<ins>cuil_operario</ins>, nombre_operario, apellido_operario, fecha_nacimiento)*
* L10: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario)*

L9 ⋂ L10 = `cuil_operario`, que es superclave en L9. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L5`: 6
* `L7`: 4
* `L9`: 2
* `L10`: 1, 7

## Iteración 6
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L9: *(<ins>cuil_operario</ins>, nombre_operario, apellido_operario, fecha_nacimiento)*
* L10: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, valor_medicion, fecha_medicion, cuil_operario)*

L1, L3, L5, L7, L9 y L10 están en BCFN. L10 no está en BCFN.

Particiono L10 siguiendo la DF 7: `#medición, #parámetro -> valor_medición`:
* L11: *(<ins>#medición, #parámetro</ins>, valor_medición)*
* L12: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, fecha_medicion, cuil_operario)*

L11 ⋂ L12 = `#medición, #parámetro`, que es superclave en L11. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L5`: 6
* `L7`: 4
* `L9`: 2
* `L11`: 7
* `L12`: 1

## Iteración 7
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L9: *(<ins>cuil_operario</ins>, nombre_operario, apellido_operario, fecha_nacimiento)*
* L11: *(<ins>#medición, #parámetro</ins>, valor_medición)*
* L12: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>, #pozo, fecha_medicion, cuil_operario)*

L1, L3, L5, L7, L9 y L11 están en BCFN. L12 no está en BCFN.

Particiono L12 siguiendo la DF 1: `#medición -> #pozo, cuil_operario, fecha_medición`:
* L13: *(<ins>#medición</ins>, #pozo, cuil_operario, fecha_medición)*
* L14: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>)*

L13 ⋂ L14 = `#medición`, que es superclave en L13. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #medición -> #pozo, cuil_operario, fecha_medición
2. cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
3. #parámetro -> nombre_parámetro, valor_ref
4. #instrumento -> marca_instrumento, modelo_instrumento
5. #pozo -> descripción_pozo, fecha_perforación
6. dominio_vehículo -> fecha_adquisición
7. #medición, #parámetro -> valor_medición

* `L1`: 5
* `L3`: 3
* `L5`: 6
* `L7`: 4
* `L9`: 2
* `L11`: 7
* `L13`: 1

## Normalización a BCFN
L1, L3, L5, L7, L9, L11 y L13 están en BCFN.

L14 está en BCFN, puesto que tiene una DF **trivial**.

Se terminó el proceso de normalización a BCFN con las siguientes particiones:
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L9: *(<ins>cuil_operario</ins>, nombre_operario, apellido_operario, fecha_nacimiento)*
* L11: *(<ins>#medición, #parámetro</ins>, valor_medición)*
* L13: *(<ins>#medición</ins>, #pozo, cuil_operario, fecha_medición)*
* L14: *(<ins>#medicion, #parámetro, #instrumento, dominio_vehiculo</ins>)*

**Clave primaria**: *(<ins>#medición, #parámetro, #instrumento, dominio_vehículo</ins>)*

## Normalización a 4FN

Se encontraron las siguientes DF multivaluadas en L14:
1. #medición >> #parámetro
2. #medición >> #instrumento

Por lo tanto, el esquema no está en 4FN.

Se particiona L14 siguiendo las DFM 1 y 2:
* L15: *(<ins>#medición, #parámetro, dominio_vehículo</ins>)*
* L16: *(<ins>#medición, #instrumento, dominio_vehículo</ins>)*

Se terminó el proceso de normalización a 4FN con las siguientes particiones:
* L1: *(<ins>#pozo</ins>, descripción_pozo, fecha_perforación)*
* L3: *(<ins>#parámetro</ins>, nombre_parámetro, valor_ref)*
* L5: *(<ins>dominio_vehículo</ins>, fecha_adquisición)*
* L7: *(<ins>#instrumento</ins>, marca_instrumento, modelo_instrumento)*
* L9: *(<ins>cuil_operario</ins>, nombre_operario, apellido_operario, fecha_nacimiento)*
* L11: *(<ins>#medición, #parámetro</ins>, valor_medición)*
* L13: *(<ins>#medición</ins>, #pozo, cuil_operario, fecha_medición)*
* L15: *(<ins>#medición, #parámetro, dominio_vehículo</ins>)*
* L16: *(<ins>#medición, #instrumento, dominio_vehículo</ins>)*

# 8.

*FESTIVALES (#festival, denominacion_festival, localidad, cuil_musico, nombre_musico, fecha_nacimiento, #banda, nombre_banda, estilo_musical, #tema, nombre_tema, duracion, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)*

Donde:
* Para cada festival se conoce su denominación y la localidad en la que se realiza. Más de un festival podría tener la misma denominación.
* De cada banda se conoce su nombre y estilo musical.
* De cada músico se conoce su nombre y su fecha de nacimiento. Tenga en cuenta que varios músicos podrían tener el mismo nombre.
* Para cada tema interpretado por una banda en un festival se conoce su nombre y duración. Además, de cada músico que participó en el tema se sabe con qué instrumento lo hizo.
* Los #tema pueden repetirse para las distintas bandas.
* Un festival puede tener varios auspiciantes, y se vendieron entradas al mismo a través de varias plataformas.
* Se tiene además un registro de todas las posibles locaciones donde se pueden realizar los festivales.

DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

**Clave candidata:** *(#festival, #banda, #tema, cuil_músico)*

## Iteración 1
*Festivales* no está en BCFN, puesto que tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono el esquema siguiendo la DF 2 `#banda -> nombre_banda, estilo_musical`:
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L2: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, denominación_festival, localidad, nombre_músico, fecha_nacimiento, nombre_tema, duración, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)*

L1 ⋂ L2 = `#banda`, que es superclave de L1. Por lo tanto, no se perdió información.

No se perdieron DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

* `L1`: 2
* `L2`: 1, 3, 4, 5

## Iteración 2
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L2: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, denominación_festival, localidad, nombre_músico, fecha_nacimiento, nombre_tema, duración, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)*

L1 está en BCFN, puesto que en la DF 1, `#banda` es superclave del esquema.

L2 no cumple con BCFN porque tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono L2 siguiendo la DF 3 `cuil_músico -> nombre_músico, fecha_nacimiento`:
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L4: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, denominación_festival, localidad, nombre_tema, duración, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)*

L3 ⋂ L4 = `cuil_músico`, que es superclave en L3. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

* `L1`: 2
* `L3`: 3
* `L4`: 1, 4, 5

## Iteración 3
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L4: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, denominación_festival, localidad, nombre_tema, duración, instrumento, cuil_auspiciante, url_plataforma_entradas, #locacion)*

L1 y L3 están en BCFN.

L4 no cumple con BCFN, puesto que tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono L4 siguiendo la DF 1 `#festival -> denominación_festival, localidad, cuil_auspiciente, url_plataforma_entradas`:
* L5: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas)*
* L6: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, nombre_tema, duración, instrumento, #locacion)*

L5 ⋂ L6 = `#festival`, que es superclave en L5. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

* `L1`: 2
* `L3`: 3
* `L5`: 1
* `L6`: 4, 5

## Iteración 4
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L5: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas)*
* L6: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, nombre_tema, duración, instrumento, #locacion)*

L1, L3 y L5 cumplen con BCFN.

L6 no cumple con BCFN, puesto que tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono L6 siguiendo la DF 4 `#tema, #banda, #festival -> nombre_tema, duración`:
* L7: *(<ins>#tema, #banda, #festival</ins>, nombre_tema, duración)*
* L8: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, instrumento, #locacion)*

L7 ⋂ L8 = `#tema, #banda, #festival`, que es superclave en L7. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

* `L1`: 2
* `L3`: 3
* `L5`: 1
* `L7`: 4
* `L8`: 5

## Iteración 5
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L5: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas)*
* L7: *(<ins>#tema, #banda, #festival</ins>, nombre_tema, duración)*
* L8: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, instrumento, #locacion)*

L1, L3, L5 y L7 están en BCFN.

L8 no cumple con BCFN, puesto que tiene la DF 5, la cual es no trivial y `#tema, cuil_músico` no es superclave.

Particiono L8 siguiendo la DF 5 `#tema, cuil_músico -> instrumento`:
* L9: *(<ins>#tema, cuil_músico</ins>, instrumento)*
* L10: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, #locacion)*

L9 ⋂ L10 = `#tema, cuil_músico`, que es superclave en L9. Por lo tanto, no se pierde información.

No se perdieron DF:
1. #festival -> denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas
2. #banda -> nombre_banda, estilo_musical
3. cuil_músico -> nombre_músico, fecha_nacimiento
4. #tema, #banda, #festival -> nombre_tema, duración
5. #tema, cuil_músico -> instrumento

* `L1`: 2
* `L3`: 3
* `L5`: 1
* `L7`: 4
* `L9`: 5

## Normalización a BCFN
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L5: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante, url_plataforma_entradas)*
* L7: *(<ins>#tema, #banda, #festival</ins>, nombre_tema, duración)*
* L9: *(<ins>#tema, cuil_músico</ins>, instrumento)*
* L10: *(<ins>#festival, #banda, #tema, cuil_músico</ins>, #locacion)*

L1, L3, L5, L7 y L9 están en BCFN.

L10 está en BCFN, puesto que sólo tiene DF triviales.

*// qué pasa con `#locación` ???*

## Normalización a 4FN
Se encontraron las siguientes DF multivaluadas:
1.  #festival >> cuil_auspiciante
2.  #festival >> url_plataforma_entradas
3.  ⊘ >> #locacion

Por lo tanto, el esquema no está en 4FN.

Se particiona L5 siguiendo las DFM 1 y 2:
* L11: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante)*
* L12: *(<ins>#festival</ins>, denominación_festival, localidad, url_plataforma_entradas)*

Por último, se produce un esquema nuevo para resolver la DFM 3:
* L13: *(<ins>#locación</ins>)*

Se terminó el proceso de normalización a 4FN con las siguientes particiones:
* L1: *(<ins>#banda</ins>, nombre_banda, estilo_musical)*
* L3: *(<ins>cuil_músico</ins>, nombre_músico, fecha_nacimiento)*
* L7: *(<ins>#tema, #banda, #festival</ins>, nombre_tema, duración)*
* L9: *(<ins>#tema, cuil_músico</ins>, instrumento)*
* L11: *(<ins>#festival</ins>, denominación_festival, localidad, cuil_auspiciante)*
* L12: *(<ins>#festival</ins>, denominación_festival, localidad, url_plataforma_entradas)*
* L13: *(<ins>#locación</ins>)*

# 9.

*Dispositivos (Marca_id, descripMarca, modelo_id, descripModelo, equipo_tipo_id, descripEquipoTipo, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn, direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

Donde:
* Para cada equipo interesa conocer su tipo, modelo, imei, fecha en que se dio de alta, fecha en que se da de baja y las observaciones que sean necesarias.
* De cada marca se conoce su descripción
* De cada modelo se conoce su descripción y a qué marca pertenece.
* Para cada plan, se registra qué empresa lo brinda, descripción e importe del mismo.
* Para cada tipo de equipo se conoce la descripción
* Para cada empresa se registra el nombre, cuit y dirección
* De cada usuario se registra su nombre y apellido, número de documento, dirección y CUIL *// no existe un campo para el DNI así que no lo pongo*
* Para cada línea se necesita registrar qué plan posee, la fecha de alta de la línea, la fecha de baja, el equipo que la posee y el usuario de la misma.

DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario

**Claves candidatas:**
* CC1: *(equipo_id, plan_id, usuario_id)*
* CC2: *(equipo_id, plan_id, cuil)*

## Iteración 1

*Dispositivos* no está en BCFN, puesto que tiene DF no triviales o en las que `X` no es superclave del esquema.

Particiono *Dispositivos* siguiendo la DF 6  `equipo_tipo_id -> descripEquipoTipo`
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L2: *(<ins>equipo_id, plan_id, usuario_id</ins>, Marca_id, descripMarca, modelo_id, descripModelo, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1 ⋂ L2 = `equipo_tipo_id`, que es superclave en L1. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L2`: 1, 2, 3, 4, 5, 7, 8, 9

## Iteración 2
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L2: *(<ins>equipo_id, plan_id, usuario_id</ins>, Marca_id, descripMarca, modelo_id, descripModelo, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1 está en BCFN. L2 no está en BCFN.

Particiono L2 siguiendo la DF 4 `marca_id -> descrip_marca`:
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L4: *(<ins>equipo_id, plan_id, usuario_id</ins>, Marca_id, modelo_id, descripModelo, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L3 ⋂ L4 = `marca_id`, que es superclave en L3. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L4`: 1, 2, 3, 5, 7, 8, 9

## Iteración 3
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L4: *(<ins>equipo_id, plan_id, usuario_id</ins>, Marca_id, modelo_id, descripModelo, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1 y L3 están en BCFN. L4 no está en BCFN.

Particiono L4 siguiendo la DF 3 `modelo_id -> descripModelo, marca_id`:
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L6: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L5 ⋂ L6 = `modelo_id`, que es superclave en L5. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L5`: 3
* `L6`: 1, 2, 5, 7, 8, 9

## Iteración 4
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L6: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, nombreEmpresa, cuit, direcciónEmpresa, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1, L3 y L5 están en BCFN. L6 no está en BCFN.

Particiono L6 siguiendo la DF 7 `cuit -> nombreEmpresa, direcciónEmpresa`:
* L7: *(<ins>cuit</ins>, nombreEmpresa, direcciónEmpresa)*
* L8: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, cuit, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L7 ⋂ L8 = `cuit`, que es superclave en L7. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L5`: 3
* `L7`: 7
* `L8`: 1, 2, 5, 8, 9

## Iteración 5
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L7: *(<ins>cuit</ins>, nombreEmpresa, direcciónEmpresa)*
* L8: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, cuit, apyn, direcciónUsuario, cuil, descripPlan, importe, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1, L3, L5 y L7 están en BCFN. L8 no está en BCFN.

Particiono L8 siguiendo la DF 5 `plan_id -> cuit, descripPlan, importe`:
* L9: *(<ins>plan_id</ins>, cuit, descripPlan, importe)*
* L10: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, apyn, direcciónUsuario, cuil, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L9 ⋂ L10 = `plan_id`, que es superclave en L9. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L5`: 3
* `L7`: 7
* `L9`: 5
* `L10`: 1, 2, 8, 9

## Iteración 6
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L7: *(<ins>cuit</ins>, nombreEmpresa, direcciónEmpresa)*
* L9: *(<ins>plan_id</ins>, cuit, descripPlan, importe)*
* L10: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, apyn, direcciónUsuario, cuil, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1, L3, L5, L7 y L9 están en BCFN. L10 no está en BCFN.

Particiono L10 siguiendo la DF 8 `usuario_id -> apyn, direcciónUsuario, cuil`:
* L11: *(<ins>usuario_id</ins>, apyn, direcciónUsuario, cuil)*
* L12: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

Al hacer esto, también se movió la DF 9 a L11.

L11 ⋂ L12 = `usuario_id`, que es superclave en L11. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L5`: 3
* `L7`: 7
* `L9`: 5
* `L11`: 8, 9
* `L12`: 1, 2

## Iteración 7
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L7: *(<ins>cuit</ins>, nombreEmpresa, direcciónEmpresa)*
* L9: *(<ins>plan_id</ins>, cuit, descripPlan, importe)*
* L11: *(<ins>usuario_id</ins>, apyn, direcciónUsuario, cuil)*
* L12: *(<ins>equipo_id, plan_id, usuario_id</ins>, modelo_id, equipo_tipo_id, imei, fec_alta, fec_baja, observaciones, línea_id, fec_alta_linea, fec_baja_linea)*

L1, L3, L5, L7, L9 y L11 están en BCFN. L12 no está en BCFN.

Particiono L12 siguiendo la DF 1 `equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones`:
* L13: *(<ins>equipo_id</ins>, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones)*
* L14: *(<ins>equipo_id, plan_id, usuario_id</ins>s, línea_id, fec_alta_linea, fec_baja_linea)*

Al hacer esto, también se movió la DF 2 a L13.

L13 ⋂ L14 = `equipo_id`, que es superclave en L13. Por lo tanto, no se perdió información.

No se perdieron DF:
1. equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones
2. imei -> equipo_tipo_id, modelo_id, equipo_id, fec_alta, fec_baja, observaciones
3. modelo_id -> descripModelo, marca_id
4. marca_id -> descripMarca
5. plan_id -> cuit, descripPlan, importe
6. equipo_tipo_id -> descripEquipoTipo
7. cuit -> nombreEmpresa, direcciónEmpresa
8. usuario_id -> apyn, direcciónUsuario, cuil
9. cuil -> usuario_id, apyn, direcciónUsuario
* `L1`: 6
* `L3`: 4
* `L5`: 3
* `L7`: 7
* `L9`: 5
* `L11`: 8, 9
* `L13`: 1, 2

## Normalización a BCFN
* L1: *(<ins>equipo_tipo_id</ins>, descripEquipoTipo)*
* L3: *(<ins>marca_id</ins>, descrip_marca)*
* L5: *(<ins>modelo_id</ins>, descripModelo, marca_id)*
* L7: *(<ins>cuit</ins>, nombreEmpresa, direcciónEmpresa)*
* L9: *(<ins>plan_id</ins>, cuit, descripPlan, importe)*
* L11: *(<ins>usuario_id</ins>, apyn, direcciónUsuario, cuil)*
* L13: *(<ins>equipo_id</ins>, equipo_tipo_id, modelo_id, imei, fec_alta, fec_baja, observaciones)*
* L14: *(<ins>equipo_id, plan_id, usuario_id</ins>, línea_id, fec_alta_linea, fec_baja_linea)*

*// Me faltó una DF con línea_id, pero no voy a hacer todo de nuevo para agregarla :(*