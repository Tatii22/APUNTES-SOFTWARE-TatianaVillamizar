# 💻 SQL 2

## 👩🏻‍💻 Consultas Avanzadas 

## 🔹 1. INNER JOIN + GROUP BY + HAVING + ORDER BY  

```sql
-- En esta consulta unos clientes con pedidos usando INNER JOIN
-- Luego agrupo por cliente para calcular la suma total de sus pedidos
-- HAVING se usa para filtrar resultados después del agrupamiento (no WHERE)
-- ORDER BY DESC muestra primero los que más gastaron

SELECT 
    c.id_cliente,
    CONCAT(c.nombre, ' ', c.apellido) AS cliente,
    SUM(p.total) AS total_gastado
FROM clientes AS c
INNER JOIN pedidos AS p ON c.id_cliente = p.id_cliente
GROUP BY c.id_cliente
HAVING total_gastado > 500
ORDER BY total_gastado DESC;
```
## 🔹 2. SUBCONSULTA EN WHERE
```sql
-- Aquí busco los clientes que tienen más pedidos que el promedio general
-- La subconsulta interna calcula el promedio de pedidos por cliente
-- HAVING se usa porque estoy comparando con una función agregada (COUNT)
-- Ejemplo clásico de subconsulta en el HAVING

SELECT 
    id_cliente, 
    COUNT(*) AS cantidad_pedidos
FROM pedidos
GROUP BY id_cliente
HAVING cantidad_pedidos > (
    SELECT AVG(contador)
    FROM (
        SELECT COUNT(*) AS contador FROM pedidos GROUP BY id_cliente
    ) AS sub
);
```
## 🔹 3. SUBCONSULTA CORRELACIONADA
```sql
-- Esta se llama correlacionada porque depende del valor de la consulta externa
-- (usa p.id_cliente dentro de la subconsulta)
-- Calcula el promedio de gasto de cada cliente y lo muestra junto a cada pedido
-- Ideal para comparar cada pedido con el comportamiento del cliente

SELECT 
    p.id_pedido,
    p.id_cliente,
    p.total,
    (
        SELECT AVG(total) 
        FROM pedidos AS p2 
        WHERE p2.id_cliente = p.id_cliente
    ) AS promedio_cliente
FROM pedidos AS p;
```
## 🔹 4. CASE + FUNCIONES AGREGADAS
```sql
-- CASE me permite clasificar resultados según condiciones
-- Aquí agrupo clientes por el total gastado y los etiqueto
-- Puedo usar BETWEEN para crear rangos fácilmente
-- Muy útil para crear segmentos sin modificar la tabla

SELECT 
    c.id_cliente,
    CONCAT(c.nombre, ' ', c.apellido) AS cliente,
    SUM(p.total) AS total_gastado,
    CASE 
        WHEN SUM(p.total) > 1000 THEN 'Cliente Premium'
        WHEN SUM(p.total) BETWEEN 500 AND 1000 THEN 'Cliente Frecuente'
        ELSE 'Cliente Nuevo'
    END AS categoria
FROM clientes AS c
JOIN pedidos AS p ON c.id_cliente = p.id_cliente
GROUP BY c.id_cliente;
```
## 🔹 5. FUNCIONES DE TEXTO Y FECHA
```sql
-- UPPER(): convierte texto a mayúsculas
-- CONCAT(): une columnas (nombre + apellido)
-- DATE_FORMAT(): cambia el formato de fecha
-- YEAR(): extrae solo el año de una fecha
-- Ideal para presentar resultados más limpios

SELECT 
    UPPER(CONCAT(c.nombre, ' ', c.apellido)) AS cliente,
    DATE_FORMAT(p.fecha_pedido, '%d-%m-%Y') AS fecha_formateada,
    p.total
FROM pedidos AS p
JOIN clientes AS c ON p.id_cliente = c.id_cliente
WHERE YEAR(p.fecha_pedido) = 2025;
```
## 🧠 PROCEDIMIENTOS ALMACENADOS (STORED PROCEDURES)
```sql
-- Los PROCEDIMIENTOS son bloques de código que se guardan en la base de datos.
--    Se usan para automatizar tareas y evitar repetir consultas largas.
--    Podemos recibir parámetros de entrada (IN), salida (OUT) o ambos (INOUT).

-- EJEMPLO 1: Crear un procedimiento para obtener el total gastado por un cliente

DELIMITER //
CREATE PROCEDURE obtener_total_cliente(IN id_cliente INT)
BEGIN
    -- Declaramos una variable local para guardar el total
    DECLARE total DECIMAL(10,2);

    -- Obtenemos la suma de todas las compras del cliente
    SELECT SUM(total_compra)
    INTO total
    FROM compras
    WHERE id_cliente_fk = id_cliente;

    -- Mostramos el resultado
    SELECT CONCAT('El cliente ', id_cliente, ' ha gastado un total de: ', total) AS Resultado;
END //
DELIMITER ;

-- 💡 NOTAS:
-- - DELIMITER se usa para cambiar el delimitador temporalmente, ya que MySQL usa ";" por defecto.
-- - "IN" indica que el parámetro es de entrada.
-- - "INTO total" guarda el resultado de la consulta dentro de la variable local.
-- - "SELECT CONCAT..." sirve para mostrar el texto con el resultado.

-- ▶️ Ejecutar el procedimiento
CALL obtener_total_cliente(3);

--  EJEMPLO 2: Procedimiento con lógica condicional (IF - ELSEIF - ELSE)

DELIMITER //
CREATE PROCEDURE verificar_descuento(IN id_cliente INT)
BEGIN
    DECLARE total DECIMAL(10,2);

    SELECT SUM(total_compra)
    INTO total
    FROM compras
    WHERE id_cliente_fk = id_cliente;

    IF total > 1000 THEN
        SELECT 'Cliente VIP - Descuento del 15%' AS Mensaje;
    ELSEIF total BETWEEN 500 AND 1000 THEN
        SELECT 'Cliente Regular - Descuento del 5%' AS Mensaje;
    ELSE
        SELECT 'Cliente Nuevo - Sin descuento' AS Mensaje;
    END IF;
END //
DELIMITER ;

-- 💬 Aquí se ve cómo usar condiciones dentro del procedimiento.
--    Muy útil para aplicar lógica de negocio.
CALL verificar_descuento(4);

-- 🧩 NOTAS GENERALES SOBRE PROCEDIMIENTOS:
-- - Son ideales para encapsular procesos complejos.
-- - Pueden contener:
--      ✅ Variables locales (DECLARE)
--      ✅ Estructuras de control (IF, CASE, WHILE, LOOP)
--      ✅ Consultas SELECT, INSERT, UPDATE, DELETE
-- - Se almacenan en el diccionario de datos del esquema.
-- - Se pueden listar con: SHOW PROCEDURE STATUS;
-- - Se eliminan con: DROP PROCEDURE nombre_procedimiento;
```
## ⏰ EVENTOS Y TRIGGERS 
```sql
-- 🚀 EVENTOS (EVENTS)
-- Los EVENTOS permiten ejecutar código SQL automáticamente en un horario o intervalo específico.
-- Sirven para automatizar tareas, por ejemplo: limpiezas, actualizaciones o reportes periódicos.

-- 🔹 Antes de crear eventos, hay que activar el programador de eventos:
SET GLOBAL event_scheduler = ON;

-- 🔹 Verificar si está activo:
SHOW VARIABLES LIKE 'event_scheduler';

-- 💡 Si dice "OFF", hay que activarlo con el comando anterior.

-- 📦 EJEMPLO 1: Crear un evento que actualiza automáticamente un campo

DELIMITER //
CREATE EVENT actualizar_clientes_inactivos
ON SCHEDULE EVERY 1 MONTH
DO
BEGIN
    UPDATE clientes
    SET estado = 'Inactivo'
    WHERE fecha_ultima_compra < DATE_SUB(CURDATE(), INTERVAL 6 MONTH);
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - "ON SCHEDULE EVERY 1 MONTH" significa que se ejecutará cada mes.
-- - "DATE_SUB(CURDATE(), INTERVAL 6 MONTH)" resta 6 meses a la fecha actual.
-- - Se usa para automatizar mantenimiento de datos.
-- - El bloque DO puede contener varias consultas.
-- - Para verlo en la lista: SHOW EVENTS;
-- - Para eliminarlo: DROP EVENT actualizar_clientes_inactivos;

-- 📦 EJEMPLO 2: Evento con fecha y hora exacta

DELIMITER //
CREATE EVENT limpiar_registros_temporales
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 DAY
DO
BEGIN
    DELETE FROM logs WHERE fecha < DATE_SUB(NOW(), INTERVAL 7 DAY);
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - Se ejecutará una sola vez, al día siguiente.
-- - Muy útil para limpiar tablas de registros o temporales.

```
## 🧩 TRIGGERS (DISPARADORES)
```sql
-- 🚀 Los TRIGGERS se ejecutan automáticamente cuando ocurre una acción en una tabla.
--    Se pueden activar antes o después de un INSERT, UPDATE o DELETE.

-- 📌 SINTAXIS BÁSICA:
-- CREATE TRIGGER nombre_trigger
-- {BEFORE | AFTER} {INSERT | UPDATE | DELETE}
-- ON nombre_tabla
-- FOR EACH ROW
-- BEGIN
--    -- Código SQL
-- END;

-- 📦 EJEMPLO 1: Trigger que registra cambios en los precios de productos

DELIMITER //
CREATE TRIGGER registrar_cambio_precio
BEFORE UPDATE ON productos
FOR EACH ROW
BEGIN
    -- Si el precio cambia, se guarda en la tabla de historial
    IF OLD.precio <> NEW.precio THEN
        INSERT INTO historial_precios (id_producto_fk, precio_anterior, precio_nuevo, fecha_cambio)
        VALUES (OLD.id_producto, OLD.precio, NEW.precio, NOW());
    END IF;
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - OLD hace referencia al valor antes del cambio.
-- - NEW al valor nuevo del registro.
-- - BEFORE UPDATE se ejecuta antes de que el cambio ocurra.
-- - Se usa para mantener trazabilidad de datos o auditorías.

-- 📦 EJEMPLO 2: Trigger para actualizar el total gastado del cliente automáticamente

DELIMITER //
CREATE TRIGGER actualizar_total_cliente
AFTER INSERT ON compras
FOR EACH ROW
BEGIN
    UPDATE clientes
    SET total_gastado = total_gastado + NEW.total_compra
    WHERE id_cliente = NEW.id_cliente_fk;
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - AFTER INSERT: se ejecuta justo después de que se inserta una nueva compra.
-- - NEW.total_compra accede al valor recién insertado.
-- - Ayuda a mantener datos sincronizados entre tablas relacionadas.


-- 📦 EJEMPLO 3: Trigger que impide borrar un producto si tiene ventas

DELIMITER //
CREATE TRIGGER evitar_borrado_producto
BEFORE DELETE ON productos
FOR EACH ROW
BEGIN
    DECLARE existe_venta INT;
    SELECT COUNT(*) INTO existe_venta
    FROM detalle_venta
    WHERE id_producto_fk = OLD.id_producto;

    IF existe_venta > 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'No se puede eliminar un producto con ventas registradas.';
    END IF;
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - SIGNAL genera un error personalizado para detener la operación.
-- - BEFORE DELETE bloquea la eliminación antes de que ocurra.
-- - Esto protege la integridad referencial.

```
```sql
-- 🧩 NOTAS FINALES
-- ✅ EVENTOS → Automatizan tareas por tiempo (tipo cron job dentro de MySQL)
-- ✅ TRIGGERS → Se activan con acciones (INSERT, UPDATE, DELETE)
-- ✅ Ambos ayudan a mantener la base de datos limpia, coherente y automatizada.
-- ✅ Siempre usa DELIMITER cuando hay bloques BEGIN/END.
-- ✅ SHOW TRIGGERS; para listar los triggers del esquema actual.
-- ✅ DROP TRIGGER nombre_trigger; para eliminarlos.
```

## ⚙️ FUNCIONES EN MYSQL
```sql
-- 🚀 Las FUNCIONES son bloques de código almacenados que devuelven un valor.
-- A diferencia de los PROCEDIMIENTOS, las funciones siempre retornan algo (RETURN).
-- Se pueden usar directamente en consultas SELECT, WHERE, ORDER BY, etc.

-- 📌 ESTRUCTURA GENERAL
-- CREATE FUNCTION nombre_funcion(parametros)
-- RETURNS tipo_dato
-- DETERMINISTIC | NOT DETERMINISTIC
-- BEGIN
--     DECLARE variables;
--     -- Lógica de la función
--     RETURN valor;
-- END;
```
```sql
-- 📦 EJEMPLO 1: Función para calcular el total de compras de un cliente

DELIMITER //
CREATE FUNCTION total_compras_cliente(id_cliente INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(10,2);

    SELECT SUM(total_compra)
    INTO total
    FROM compras
    WHERE id_cliente_fk = id_cliente;

    RETURN IFNULL(total, 0); -- Evita retornar NULL si el cliente no tiene compras
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - DETERMINISTIC indica que la función siempre devuelve el mismo resultado para los mismos datos.
-- - IFNULL(total, 0) reemplaza los valores nulos por 0.
-- - Se usa RETURN para devolver el valor final.
-- - Para llamarla: SELECT total_compras_cliente(3);

-- 📦 EJEMPLO 2: Función que clasifica a un cliente según lo que ha gastado

DELIMITER //
CREATE FUNCTION clasificar_cliente(id_cliente INT)
RETURNS VARCHAR(30)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(10,2);
    DECLARE categoria VARCHAR(30);

    SELECT SUM(total_compra)
    INTO total
    FROM compras
    WHERE id_cliente_fk = id_cliente;

    IF total >= 1000 THEN
        SET categoria = 'Cliente VIP';
    ELSEIF total >= 500 THEN
        SET categoria = 'Cliente Regular';
    ELSE
        SET categoria = 'Cliente Nuevo';
    END IF;

    RETURN categoria;
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - Aquí combinamos lógica condicional dentro de una función.
-- - RETURN devuelve un texto dependiendo del valor total.
-- - Esta función se puede usar directamente en una consulta:
--   SELECT nombre, clasificar_cliente(id_cliente) AS Categoria FROM clientes;

-- 📦 EJEMPLO 3: Función que calcula el promedio de precios por categoría

DELIMITER //
CREATE FUNCTION promedio_categoria(id_categoria INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE promedio DECIMAL(10,2);

    SELECT AVG(precio)
    INTO promedio
    FROM productos
    WHERE id_categoria_fk = id_categoria;

    RETURN IFNULL(promedio, 0);
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - Se puede combinar con agregaciones como AVG, COUNT, SUM.
-- - Muy útil para análisis o reportes personalizados.
-- - Para usarla: SELECT promedio_categoria(2);

-- 📦 EJEMPLO 4: Función con parámetros y cálculo matemático

DELIMITER //
CREATE FUNCTION calcular_descuento(precio DECIMAL(10,2), porcentaje DECIMAL(5,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN precio - (precio * (porcentaje / 100));
END //
DELIMITER ;

-- ✏️ ANOTACIONES:
-- - Retorna el precio final aplicando el porcentaje de descuento.
-- - Se puede usar en cualquier consulta:
--   SELECT nombre, calcular_descuento(precio, 10) AS Precio_Con_Descuento FROM productos;

```
```sql
-- 🧩 NOTAS GENERALES DE FUNCIONES:
-- ✅ Las funciones siempre devuelven un valor con RETURN.
-- ✅ Pueden usarse dentro de SELECT, WHERE, HAVING, etc.
-- ✅ No pueden modificar datos (no se permiten INSERT, UPDATE o DELETE dentro).
-- ✅ Se pueden listar con: SHOW FUNCTION STATUS;
-- ✅ Se eliminan con: DROP FUNCTION nombre_funcion;
-- ✅ Usa DETERMINISTIC si la función depende solo de los datos (no del tiempo o variables externas).
-- ✅ Siempre recuerda usar DELIMITER cuando hay bloques BEGIN/END.

```
