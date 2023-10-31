# 2.Listar DNI, nombre y apellido de todos los clientes ordenados por DNI en forma ascendente. Realice la consulta en ambas bases. ¿Qué diferencia nota en cuanto a performance? ¿Arrojan los mismos resultados? ¿Qué puede concluir en base a las diferencias halladas?

```sql
--reparaciones
SELECT dniCliente, nombreApellidoCliente
FROM cliente
ORDER BY dniCliente

-- reparaciones_dn
SELECT dniCliente, nombreApellidoCliente
FROM reparacion
ORDER BY dniCliente
```

|                   | Performance | Cant. resultados |
| ----------------- | ----------- | ---------------------- |
| `reparaciones`    | 0,016 segundos | 20000 |
| `reparaciones_dn` | 0,063 segundos | 162252 |

La consulta sobre `reparaciones` retorna una sola tuplas para cada DNI y nombre, mientras que `reparaciones_dn` tiene tuplas con el DNI y nombre repetidos. Con esto podemos concluir que una BD normalizada no sólo es más eficiente, sino que también es más consistente con sus resultados.

# 3. Hallar aquellos clientes que para todas sus reparaciones siempre hayan usado su tarjeta de crédito primaria (nunca la tarjeta secundaria). Realice la consulta en ambas bases.

```sql
-- reparaciones
SELECT c.dniCliente, c.nombreApellidoCliente
FROM cliente c
WHERE NOT EXISTS (
	SELECT *
	FROM Reparacion r
    WHERE r.dniCliente = c.dniCliente AND r.tarjetaReparacion = c.tarjetaSecundaria
)

-- reparaciones_dn
SELECT *
FROM reparacion r
WHERE NOT EXISTS (
	SELECT *
    FROM reparacion r2
    WHERE r.dniCliente = r2.dniCliente AND r.tarjetaSecundaria = r2.tarjetaReparacion
)
```

# 4. Crear una vista llamada ‘sucursalesPorCliente’ que muestre los dni de los clientes y los códigos de sucursales de la ciudad donde vive el cliente. Cree la vista en ambas bases.

```sql
-- reparacion
CREATE VIEW sucursalesPorCliente AS
    SELECT c.dniCliente, s.codSucursal
    FROM cliente c INNER JOIN sucursal s ON  c.ciudadCliente = s.ciudadSucursal
    ORDER BY c.dniCliente;

-- reparacion_dn
CREATE VIEW sucursales AS
	SELECT DISTINCT codSucursal, ciudadSucursal
	FROM reparacion;

CREATE VIEW clientes AS
	SELECT DISTINCT dniCliente, ciudadCliente
	FROM reparacion;

CREATE VIEW sucursalesPorCliente AS
	SELECT dniCliente, codSucursal
    FROM sucursales INNER JOIN clientes ON ciudadCliente = ciudadSucursal
    ORDER BY dniCliente;
```

# 5. En la base normalizada, hallar los clientes que dejaron vehículos a reparar en todas las sucursales de la ciudad en la que viven

## a. Realice la consulta sin utilizar la vista creada anteriormente

```sql
SELECT dniCliente FROM cliente c
WHERE (
	SELECT COUNT(codSucursal) FROM sucursal s
    WHERE c.ciudadCliente = s.ciudadSucursal)
= (
	SELECT COUNT(DISTINCT s.codSucursal) FROM reparacion r
    INNER JOIN sucursal s ON r.codSucursal = s.codSucursal
    WHERE c.dniCliente = r.dniCliente
    AND c.ciudadCliente = s.ciudadSucursal
)
ORDER BY dniCliente
LIMIT 100;
```

## b. Realice la consulta utilizando la vista creada anteriormente
```sql
SELECT dniCliente FROM cliente c
WHERE (
	SELECT COUNT(codSucursal) FROM sucursalesPorCliente s
    WHERE c.dniCliente = s.dniCliente)
= (
	SELECT COUNT(DISTINCT s.codSucursal) FROM reparacion r
    INNER JOIN sucursal s ON r.codSucursal = s.codSucursal
    WHERE c.dniCliente = r.dniCliente
    AND c.ciudadCliente = s.ciudadSucursal
)
ORDER BY dniCliente
LIMIT 100;```