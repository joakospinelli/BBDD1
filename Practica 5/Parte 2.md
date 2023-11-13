# 3. Competencias

**COMPETENCIAS** (#nadador, nombre_nadador, fecha_nacimiento, #auspiciante, razon_social, domicilio_auspiciante, #competencia, nombre_competencia, fecha_competencia, lugar_competencia, #prueba, descripción_prueba, tiempo, puesto)

Donde:
* Para cada prueba de una competencia realizada por un nadador se registra su tiempo y el puesto que obtuvo. Tenga en cuenta que un mismo puesto en una prueba puede ser compartido por distintos nadadores.
* De los nadadores se conoce su nombre y fecha de nacimiento.
* De cada prueba se conoce su descripción. Y una misma prueba puede llevarse a cabo en diferentes competencias. Los #prueba son únicos en el sistema.
* De las competencias se conoce su nombre, fecha y lugar de realización.
* Cada competencia es auspiciada por varios auspiciantes, de los que se conoce su razón social y domicilio. Tenga en cuenta que puede haber más de un auspiciante con la misma razón social.

**Dependencias funcionales:**
1. #competencia, #prueba, #nadador -> tiempo, puesto
2. #nadador -> nombre_nadador, fecha_nacimiento
3. #prueba -> descripción_prueba
4. #competencia -> nombre_competencia, fecha_competencia, lugar_competencia
5. #auspiciante -> razon_social, domicilio_auspiciante

**CC**: { #competencia, #nadador, #prueba, #auspiciante }

## Paso 1

El esquema no está en BCFN, ya que tiene al menos una DF no trivial o en la que X no es superclave del esquema.

Particiono el esquema siguiendo la DF 2:

* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L2: (<ins>#competencia, #nadador, #auspiciante, #prueba</ins>, razon_social, domicilio_auspiciante, nombre_competencia, fecha_competencia, lugar_competencia, descripción_prueba, tiempo, puesto)

**Se perdió información?** No, ya que L1 ⋂ L2 = `#nadador`, que es superclave en L1.

**Se perdieron DF?** No, ya que las DF son válidas en las siguientes particiones:
* L1: DF2
* L2: DF1, DF3, DF4, DF5

## Paso 2

L2 no está en BCFN, ya que tiene la menos una DF no trivial o en la que X no es superclave del esquema.

Particiono L2 siguiendo la DF 3:

* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L3: (<ins>#prueba</ins>, descripción_prueba)
* L4: (<ins>#competencia, #nadador, #auspiciante, #prueba</ins>, razon_social, domicilio_auspiciante, nombre_competencia, fecha_competencia, lugar_competencia, tiempo, puesto)

**Se perdió información?** No, ya que L3 ⋂ L4 = `#prueba`, que es superclave en L4.

**Se perdieron DF?** No, ya que las DF son válidas en las siguientes particiones:
* L1: DF2
* L3: DF3
* L4: DF1, DF4, DF5

## Paso 3

L4 no está en BCFN, ya que tiene al menos una DF no tirvial o en la que X no es superclave.

Particiono L4 siguiendo la DF 5:

* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L3: (<ins>#prueba</ins>, descripción_prueba)
* L5: (<ins>#auspiciante</ins>, razón_social, domicilio_auspiciante)
* L6: (<ins>#competencia, #nadador, #auspiciante, #prueba</ins>, nombre_competencia, fecha_competencia, lugar_competencia, tiempo, puesto)

**Se perdió información?** No, ya que L5 ⋂ L6 = `#auspiciante`, que es superclave en L5.

**Se perdieron DF?** No, ya que las DF son válidas en las siguientes
particiones:
* L1: DF2
* L3: DF3
* L5: DF5
* L6: DF1, DF4

## Paso 3

L6 no está en BCFN, ya que tiene al menos una DF no trivial o en la que X no es superclave del esquema.

Particiono L6 siguiendo la DF 4:

* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L3: (<ins>#prueba</ins>, descripción_prueba)
* L5: (<ins>#auspiciante</ins>, razón_social, domicilio_auspiciante)
* L7: (<ins>#competencia</ins>, nombre_competencia, fecha_competencia, lugar_competencia)
* L8: (<ins>#competencia, #nadador, #auspiciante, #prueba</ins>, tiempo, puesto)

**Se perdió información?** No, ya que L7 ⋂ L8 = `#competencia`, que es superclave en L7.

**Se perdieron DF?** No, ya que las DF son válidas en las siguientes particiones:
* L1: DF2
* L3: DF3
* L5: DF5
* L7: DF4
* L8: DF1

## Paso 4

L8 no está en BCFN, ya que tiene una DF en la que X no es superclave.

Particiono L8 siguiendo la DF 1:

* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L3: (<ins>#prueba</ins>, descripción_prueba)
* L5: (<ins>#auspiciante</ins>, razón_social, domicilio_auspiciante)
* L7: (<ins>#competencia</ins>, nombre_competencia, fecha_competencia, lugar_competencia)
* L9: (<ins>#competencia, #nadador, #prueba</ins>, tiempo, puesto)
* L10: (<ins>#competencia, #nadador, #auspiciante, #prueba</ins>)

**Se perdió información?** No, ya que L9 ⋂ L10 = `#competencia, #nadador, #puesto`, que es superclave en L9.

**Se perdieron DF?** No, ya que las DF son válidas en las siguientes particiones:

* L1: DF2
* L3: DF3
* L5: DF5
* L7: DF4
* L9: DF1

Finalmente el esquema se encuentra en BCFN, habiéndose resuelto todas las dependencias funcionales no triviales.

**Clave primaria:** { <ins>#nadador, #competencia, #prueba, #auspiciante</ins> }

## Normalización a 4FN

Se encontraron las siguientes dependencias multivaluadas:
1. #competencia, #prueba >> #nadador
2. #competencia >> #auspiciante

Por lo tanto, el esquema no está en 4FN.

Particiono el esquema siguiendo las DM:
* L1: (<ins>#nadador</ins>, nombre_nadador, fecha_nacimiento)
* L3: (<ins>#prueba</ins>, descripción_prueba)
* L5: (<ins>#auspiciante</ins>, razón_social, domicilio_auspiciante)
* L7: (<ins>#competencia</ins>, nombre_competencia, fecha_competencia, lugar_competencia)
* L9: (<ins>#competencia, #nadador, #prueba</ins>, tiempo, puesto)
* L11: (<ins>#competencia, #prueba, #nadador</ins>)
* L12: (<ins>#competencia, #auspiciante</ins>)

# 4. Instalaciones

**INSTALACIONES** (idCuidador, nyAp, cuilCuidador, idVivero, nombreVivero, mtrCuadradosVivero, tempPromedioVivero, idPlanta, nombrePlanta, idEspecie, nombreEspecie, quimicoPlanta, consultorVivero)

Donde:
* El idVivero es un identificador único que no se repite para diferentes viveros. Del vivero se conoce su nombre (diferentes viveros pueden tener el mismo nombre), los metros cuadrados que ocupa, la temperatura promedio que debe mantener y el cuidador responsable del mismo.
* Del cuidador se conoce su nombre y apellido y el cuil. El idCuidador no se repite para diferentes cuidadores.
* Un mismo cuidador (idCuidador) puede cuidar diversos viveros. Tener en cuenta que un vivero tiene solamente un cuidador responsable asignado.
* El idPlanta es único. Por ejemplo, una planta es el helecho. De cada planta se conoce el nombre de la planta y la especie a la que pertenece.
* El idEspecie es único. Un ejemplo de especie es el árbol. Cada planta pertenece a una única especie y a una especie pertenecen diversas plantas.
* A cada planta en un vivero (por ejemplo: helecho en el vivero 1) se le aplica un conjunto de químicos. Los mismos químicos se pueden aplicar a plantas de diferentes viveros y a diferentes plantas en el mismo vivero.
* Cada vivero tiene diversos consultores de viveros (consultorVivero), que son quienes asesoran ante dudas eventuales. El mismo consultor puede asesorar en diversos viveros.

**Dependencias funcionales:**
1. idVivero -> nombreVivero, mtrCuadradosVivero, tempPromedioVivero, idCuidador
2. idCuidador -> nyAp, cuilCuidador
3. cuilCuidador -> idCuidador, nyAp
4. idPlanta -> nombre, idEspecie
5. idEspecie -> nombreEspecie

**CC**:
* { idVivero, idCuidador, idPlanta, químicoPlanta, consultorVivero }
* { idVivero, cuilCuidador, idPlanta, químicoPlanta, consultorVivero }

## Paso 1

El esquema no está en BCFN, ya que hay al menos una DF no trivial o en la que X no es superclave del esquema.

Particiono el esquema siguiendo la DF 5:

* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L2: (<ins>idCuidador, idVivero, idPlanta, idEspecie, cuilCuidador, quimicoPlanta, consultorVivero</ins>, nyAp, nombreVivero, mtrCuadradosVivero, tempPromedioVivero, nombrePlanta)

**Se perdió información?** No, ya que L1 ⋂ L2 = `idEspecie`, que es superclave en L1.

**Se perdieron DF?** No, ya que las DF siguen valiendo en las siguientes particiones:

* L1: DF5
* L2: DF1, DF2, DF3, DF4

## Paso 2

L2 no está en BCFN, ya que hay al menos una DF no trivial o en la que X no es superclave.

Particiono L2 siguiendo la DF 4:

* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L3: (<ins>idPlanta</ins>, nombrePlanta, nombreEspecie)
* L4: (<ins>idCuidador, idVivero, idPlanta, cuilCuidador, quimicoPlanta, consultorVivero</ins>, nyAp, nombreVivero, mtrCuadradosVivero, tempPromedioVivero)

**Se perdió información?** No, ya que L3 ⋂ L4 = `idPlanta`, que es superclave en L3.

**Se perdieron DF?** No, ya que las DF siguen valiendo en las siguientes particiones:

* L1: DF5
* L3: DF4
* L4: DF1, DF2, DF3

## Paso 3

L4 no está en BCFN, ya que tiene al menos una DF no trivial o en la que X no es superclave.

Particiono L4 siguiendo la DF 3:

* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L3: (<ins>idPlanta</ins>, nombrePlanta, nombreEspecie)
* L5: (<ins>idCuidador</ins>, nyAp, cuilCuidador)
* L6: (<ins>idCuidador, idVivero, idPlanta, quimicoPlanta, consultorVivero</ins>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero)

Al hacer esto, también se movió la DF 2 al esquema L5, y sigue siendo válida ya que tanto `idCuidador` como `cuilCuidador` son claves candidatas.

**Se perdió información?** No, ya que L5 ⋂ L6 = `idCuidador`, que es superclave en L6.

**Se perdieron DF?** No, ya que las DF siguen valiendo en las siguientes particiones:

* L1: DF5
* L3: DF4
* L5: DF2, F3
* L6: DF1

## Paso 4

L6 no está en BCFN, ya que aún tiene una DF en la que X no es superclave del esquema.

Particiono L6 siguiendo la DF 1:

* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L3: (<ins>idPlanta</ins>, nombrePlanta, nombreEspecie)
* L5: (<ins>idCuidador</ins>, nyAp, cuilCuidador)
* L7: (<ins>idVivero</ins>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero)
* L8: (<ins>idCuidador, idVivero, idPlanta, quimicoPlanta, consultorVivero</ins>)

**Se perdió información?** No, ya que L7 ⋂ L8 = `idVivero`, que es superclave en L7.

**Se perdieron DF?** No, ya que las DF siguen valiendo en las siguientes particiones:

* L1: DF5
* L3: DF4
* L5: DF2, F3
* L7: DF1

**Clave primaria:** (<ins>idCuidador, idVivero, idPlanta, químicoPlanta, consultorVivero</ins>)

## Normalización a 4FN

Se encontraron las siguientes dependencias multivaluadas:

1. idCuidador >> idVivero
2. idVivero, idPlanta >> quimicoPlanta
3. consultorVivero >> idVivero

Por lo tanto, el esquema no está en 4FN.

Particiono el esquema siguiendo la DM 2:
* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L3: (<ins>idPlanta</ins>, nombrePlanta, nombreEspecie)
* L5: (<ins>idCuidador</ins>, nyAp, cuilCuidador)
* L7: (<ins>idVivero</ins>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero)
* L9: (<ins>idCuidador, idVivero</ins>)
* L11: (<ins>idVivero, idPlanta, quimicoPlanta</ins>)
* L12: (<ins>idCuidador, idVivero, consultorVivero</ins>)

L12 aún no cumple con 4FN, puesto que sigue teniendo las DM 1 y 2, que son no triviales.

Particiono L10 siguiendo las DM 1 y 2:

* L13: (<ins>idCuidador, idVivero</ins>)
* L14( <ins>idVivero, consultorVivero</ins>)

De esta manera, el esquema termina en 4FN con las siguientes particiones:

* L1: (<ins>idEspecie</ins>, nombreEspecie)
* L3: (<ins>idPlanta</ins>, nombrePlanta, nombreEspecie)
* L5: (<ins>idCuidador</ins>, nyAp, cuilCuidador)
* L7: (<ins>idVivero</ins>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero)
* L9: (<ins>idCuidador, idVivero</ins>)
* L11: (<ins>idVivero, idPlanta, quimicoPlanta</ins>)
* L13: (<ins>idCuidador, idVivero</ins>)
* L14( <ins>idVivero, consultorVivero</ins>)