# 1. Análisis de un Modelo ER

<img src="./img/parte 1/ej1.png">

## a. En este modelo cada período de exposición contiene múltiples cuadros en museos. ¿Qué parte del modelo indica esto? ¿Cómo la modificaría para que cada período fuese exclusivo de cada cuadro expuesto en un museo?

Esto está indicado por las cardinalidades de la relación `expuesto` entre la agregación Cuadro-Museo y Período.

Para modificarlo y que haya un cuadro por período, cambiaría la cardinalidad de la agregación, para que sólo pueda haber una en todo el período.

<img src="./img/parte 1/ej1a.png">

## b. Si los cuadros se expusieron en un solo período dentro de cada museo ¿cómo ajustaría el modelo para reflejar esto?

<img src="./img/parte 1/ej1b.png">

## c. Ajuste el modelo para representar museos de dos tipos: de arte contemporáneo, con fecha de inauguración, país, director, curador a cargo y movimiento artístico; y de arte en general, del cual se conoce una fecha estimada de inauguración, país, director, restaurador principal y datos históricos. De los datos históricos se registra un año y una descripción histórica, por ejemplo que una pintura famosa se exhibió por primera vez allí en un año determinado.

<img src="./img/parte 1/ej1c.png">

# 2. Verdadero/Falso. Justificar:

## a. En una especialización, la entidad padre no modela datos que realmente existan, sino que sirve para representar los aspectos comunes de las entidades hijas.

Falso. La especialización genera un subconjunto de entidades a partir de una entidad de nivel mayor. La entidad de nivel mayor puede seguir usándose.

## b. En una agregación, la cardinalidad mínima debe ser mayor a cero.

Falso. La única restricción de cardinalidades de la agregación es que las entidades que la conforman deben tener una cardinalidad **máxima** mayor a 1.

## c. Una entidad puede no tener un atributo identificador en el modelo ER

Falso. Todas las entidades deben tener un identificador que las diferencie de otras dentro de su mismo grupo de entidades.

## d. No es correcto modelar atributos en las relaciones en un modelo ER

Falso. Es posible agregar atributos en las relaciones en el modelo ER. sin embargo, estas desaparecerán cuando se pase a un modelo relacional.

# 3. Verdadero/Falso. Justificar:

<img src="./img/parte 1/ej3.png">

El Estado Nacional implementó distintos subsidios destinados a sectores productivos. Cada subsidio tiene un nombre y un monto asignado.

Para cada subsidio se realiza una liquidación mensual, de la cual se registra a qué mes y año corresponde, el total gastado y la fecha de realización. En esta liquidación, a cada beneficiario del subsidio se le liquida un monto, el cual dependerá de la situación del beneficiario. Un beneficiario puede ser una persona Jurídica o Física, y en el caso de la persona física, debe estar inscripta en el monotributo. De cada beneficiario se conoce la actividad económica en la cual se encuentra inscripto y su cuil o cuit que lo identifica. De las personas jurídicas se conoce la razón social, provincia, departamento, localidad y cantidad de empleados. De las personas físicas se conoce nombre y apellido,provincia, departamento, localidad y categoría del monotributo.

Para el diagrama de Entidades y Relaciones propuesto en este inciso responda si las siguientes afirmaciones son V o F. Justificar:

## a. La relación `tiene` está mal definida, ya que debería ser entre `persona` y `categoría_monotributo`.

Falso. Es correcto que la relación sea entre `física` y `categoría_monotributo` puesto que sólo esa especialización de `Persona` puede inscribirse al monotributo.

## b. La relación `realiza` esta bien definida, ya que todas las personas realizan actividades.

Verdadero. La entidad `Actividad` se relaciona con `Persona` puesto que todas las entidades hoja de su jerarquía pueden realizar actividades.

## c. La jerarquía de `Persona` representa correctamente la problemática.

Falso. La jerarquía debería ser una generalización, puesto que no se pueden modelar datos de `Persona` que no sean personas físicas o jurídicas.

## d. La relación `pertenece` está mal definida, ya que no puede haber atributos en las relaciones.

Falso. Las relaciones pueden tener atributos.

## e. La agregación de la relación `posee` está correctamente definida ya que con una relación uno a muchos se puede agregar.

Falso. Las relaciones de agregación deben ser "muchos a muchos".

## f. Con este diseño es posible conocer el saldo disponible del subsidio para futuras liquidaciones.

Verdadero. Cada liquidación contiene la fecha en la que se realizó, y la persona almacena todas las liquidaciones previas. A partir de consultar a ese historial se puede obtener el saldo disponible.

## g. El modelo no tiene redundancia de datos.

Falso. Se produce una redundancia de datos en la relación `pertenece` con la agregación conformada por el `Subsidio`, puesto que tanto la relación como esta última entidad tienen un atributo `monto`.

# 4. Análisis de un modelo de E/R

Dado el siguiente modelo ER sobre vendedores que trabajan en locales:

<img src="./img/parte 1/ej4.png">

## a. En qué casos modelaría un atributo `fecha _de_ingreso` en la relación `se_emplea` –entre Vendedor y Local - como se muestra en la variante “A”?

Cuando la fecha de ingreso del vendedor depende del horario en el que trabaje.

## b. ¿En qué casos haría falta modelar una entidad Fecha de ingreso relacionada con la agregación Vendedor Local como se muestra en la parte llamada B en el modelo?

Cuando la fecha de ingreso sea independiente del horario; por ejemplo, si un vendedor puede cambiar de horario varias veces sin cambiar la fecha de ingreso.

## c. ¿Qué se está modelando con Horario cuando está la agregación? Indíquelo agregando la cardinalidad correspondiente.

Se está modelando el horario que realiza un vendedor en un local determinado. Considerando que un vendedor hace un solo horario y puede trabajar en más de un lugar, la cardinalidad de horario sería (1,1) y la de la agregación sería (1,N)

# 5. Verdadero/Falso. Transformación Modelo ER a Relacional

<img src="./img/parte 1/ej5.png">

semestre(<ins>#semestre</ins>, año, nro)

curso(<ins>código</ins>, nombre, reseña)

profesor(<ins>cuil</ins>, nyap, fecha_nac, fecha_ingreso)

info_curso(<ins>idic</ins>, fecha_comienzo, aula, día_semana, hora)

brinda(<ins>#semestre, código</ins>)

tiene(<ins>#semestre, código, idic</ins>)

dicta(idic, <ins>cuil</ins>)

## a. La relación `brinda` tiene los atributos correspondientes y su clave está bien definida

Verdadero.

## b. La relación `tiene` tiene los atributos correspondientes y su clave está bien definida

Verdadero.


## c. La relación `dicta` tiene los atributos correspondientes y su clave está bien definida

Falso. La clave debería ser `idic`, puesto que ´info_curso´ tiene una cardinalidad (1,N).

## d. La relación `tiene` no debería existir y los identificadores de la agregación deberían estar en `InfoCurso`.

Verdadero? Es una opción válida.

## e. La relación `dicta` no debería existir y los atributos de Profesor deberían estar en `InfoCurso`.

Falso. Una posible solución sería eliminar la relación y tener en `infoCurso` una clave foránea que represente al Profesor, pero al eliminar la entidad Profesor se está perdiendo la individualidad de dichas entidades.

