**RACK_DEPOSITO** (<ins>id_rack_deposito</ins>, ubicacion, id_producto, cantidad_productos_max)

**PRODUCTOS** (<ins>id_producto</ins>, nombre, precio, stock)

**ITEM** (<ins>id_item</ins>, id_producto, id_venta, cantidad)

**VENTA** (<ins>id_venta</ins>, numero_factura, fecha_venta, precio_total, nombre_cliente)

# 1. Trigger de actualización de Stock

Escriba un Trigger en SQL de modo que con cada nueva venta registrada se actualice el stock disponible de los productos vendidos. Suponga que la generación de una venta y sus respectivos ítems se realiza dentro de forma transaccional en un store procedure.

```sql
CREATE TRIGGER actualizar_stock
AFTER INSERT ON ITEM FOR EACH ROW
BEGIN
    UPDATE PRODUCTOS SET stock = (stock - NEW.cantidad)
    WHERE id_producto = NEW.id_producto
END;
```

# 2. Vista para aplicación de deposito

Crea dicha vista donde se liste el id, nombre y stock disponible de los productos, además del stock maximo que se puede tener de cada uno. Esto último es la suma de los stock maximos de los lugares del depósito asignados a este producto.

```sql
CREATE VIEW app_deposito AS
    SELECT p.id_producto, p.nombre, p.stock AS stock_actual, SUM(rd.cantidad_productos_max) AS stock_maximo
    FROM PRODUCTOS p, RACK_DEPOSITO rd
    GROUP BY p.id_producto;
```

# 3. Store Procedure de actualización de stock

Se dispone en la base de datos de una tabla con el siguiente formato:

**PRODUCTOS_REPOSICION** (id_producto, nombre, stock, stock_faltante)

Crea un stored procedure que recibe como parámetro un stock mínimo y, dentro de una transacción, recorra los productos e inserte en la tabla anteriormente mencionada los ids y nombre aquellos productos que se tenga stock por debajo la cantidad mínima, además del stock actual y del faltante para completar el stock máximo. Puede utilizar la vista creada anteriormente.

```sql
CREATE PROCEDURE actualizar_stock(IN stock_min INT)
BEGIN

    DECLARE salir INT DEFAULT 0;
    DECLARE id_act INT;
    DECLARE nombre_act VARCHAR;
    DECLARE stock_act INT;
    DECLARE stock_max INT;

    DECLARE recorrer CURSOR FOR
        SELECT * FROM app_deposito;

    DECLARE CONTINUE HANDLER FOR NOT FOUND SET salir = 1;

    START TRANSACTION
    
        OPEN recorrer;

        agregar: LOOP
            FETCH NEXT FROM recorrer INTO id_act, nombre_act, stock_act, stock_max;

            IF salir = 1 THEN
                LEAVE agregar;
            END IF;

            IF stock_act < stock_min THEN
                INSERT INTO PRODUCTOS_REPOSICIÓN (id_producto, nombre, stock, stock_faltante)
                VALUES (
                    id_act,
                    nombre_act,
                    stock_act,
                    stock_max - stock_act
                );
        END LOOP;

        CLOSE recorrer;
    COMMIT;
END;
```