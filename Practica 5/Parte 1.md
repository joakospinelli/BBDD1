# 1. Pagos de tarjetas de crédito

Se quiere realizar un sistema para el seguimiento de pagos de tarjetas de crédito.

Una tarjeta pertenece a un titular, de quien se sabe el nombre completo, CUIL, teléfono y fecha de nacimiento. El titular puede ser cliente de varios bancos, y en cada banco puede tener una o más tarjetas. De los bancos se conoce su CUIT y la razón social.

De la tarjeta en sí se conoce el número, fechas de emisión y vencimiento y CVV (código validador), el titular y el banco que la emitió.

Con una tarjeta se pueden realizar compras, de las cuales se sabe el monto, la fecha y el número de autorización. Las compras pueden ser de dos tipos: en un pago o en cuotas. De las compras en cuotas se sabe además el interés y el número de cuotas. Además, de cada cuota se guarda el vencimiento y el monto.

Los pagos se registran una vez por mes, y de cada pago de una tarjeta se sabe cuáles compras se pagaron, y en el caso de las compras en cuotas, se sabe también qué cuota se pagó.

<img src="./img/Parte 1/ej1.png"/>

*// aunque "EnUnPago" no tenga atributos adicionales, conviene hacer una generalización en vez de una especialización para separar las relaciones con "Pago"*

**TITULAR** (<ins>CUIL</ins>, nombre_completo, fecha_nac, teléfono)

**BANCO** (<ins>CUIT</ins>, razón_social)

**ESCLIENTE** (<ins>CUIT, CUIL</ins>)

**TARJETA** (<ins>#tarjeta</ins>, CVV, fecha_emisión, fecha_vence)

**TIENE** (<ins>#tarjeta</ins>, CUIL, CUIT)

**COMPRA** (<ins>#compra</ins>, fecha, monto)

**ENUNPAGO** (<ins>#compra</ins>)

**ENCUOTAS** (<ins>#compra</ins>, cant_cuotas, interés)

**CUOTA** (<ins>#cuota</ins>, fecha_vence, monto)

**SEDIVIDE** (<ins>#cuota</ins>, #compra)

**REALIZA** (<ins>#compra</ins>, #tarjeta)

**PAGO** (<ins>#pago</ins>, mes)

**PAGA** (<ins>#compra</ins>, #pago)

**PAGACUOTA** (<ins>#cuota</ins>, #pago, nro_cuota)

# 2.  Sistema de Aeropuerto

En un aeropuerto se está efectuando el diseño de una base de datos que permita llevar un registro de los aviones, pilotos y demás empleados que en él trabajan. Cada avión posee un nombre, fecha del primer vuelo y número de registro, y tiene un modelo asociado. Cada avión se guarda en un hangar determinado. Cada modelo de avión tiene un número de modelo, una capacidad y un peso, mientras que de un hangar se conoce su número, capacidad y ubicación. Tenga en cuenta que en un hangar se pueden guardar varios aviones.

Para el caso de los aeroplanos (particulares), es necesario conocer los datos del dueño (que será una persona), la fecha de compra por parte de su dueño y los empleados del aeropuerto que hacen el mantenimiento del mismo. Cada empleado de mantenimiento puede realizar diferentes tareas y la misma tarea puede ser realizada por diversos empleados. Para cada tarea de un empleado de mantenimiento, se conoce el horario en el que la realiza, teniendo en cuenta, que un empleado puede realizar la misma tarea en diferentes horarios.

El reporte de servicios a un aeroplano se entrega continuamente, por lo que es necesario llevar un registro de las fechas en que se hace el mantenimiento, el número de horas incurridas y el servicio realizado y los empleados de mantenimiento responsables del mismo.

Por otro lado, un piloto está autorizado para volar ciertos modelos de avión, dependiendo de la licencia que tenga. Cada empleado, en general, gana un sueldo fijo y, en el caso específico de los pilotos se agregan adicionales por horas de vuelo registradas en un mes. Para esto, cada vez que un piloto realiza un vuelo, se registra en su libro la fecha, las horas de vuelo y el avión.

<img src="./img/Parte 1/ej2.png">

*// de este no voy a hacer el lógico zzz*
