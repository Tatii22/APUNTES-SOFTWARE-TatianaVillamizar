# MySQL I
---
# Guía: Usar MySQL en Docker

## 1. Verificar que Docker esté instalado

- En tu terminal o PowerShell, ejecuta:
```bash
  docker --version
  ```
- Si devuelve la versión, Docker está instalado y listo para usar.

## 2. Descargar la imagen de MySQL

Docker funciona con imágenes. Para MySQL, puedes usar la oficial:
```bash
  docker pull mysql:8.0
```
## 3. Crear y correr un contenedor de MySQL
Para iniciar MySQL en un contenedor:
```bash
docker run --name mysql_container -e MYSQL_ROOT_PASSWORD=admin -p 3309:3306 -d mysql:8.4.3
```
## 4. Comandos útiles para manejar contenedores
```bash
docker ps //Listado de contenderos que estan en ejecucion
docker ps -a //Listado de todos los contenedores
docker exec -it mysql_container mysql -h localhost -u root -P 3306 -p //Para ingresar a mysql dentro del contenedor
```
---
# Comandos SQL 

## 1. Bases de datos
- **CREATE DATABASE** → crea base si no existe  
- **USE** → seleccionar base de datos  
- **DROP DATABASE** → eliminar base  

## 2. Tablas
- **CREATE TABLE** → crear tabla  
  - **PRIMARY KEY** → identificador único  
  - **UNIQUE** → valores únicos  
  - **FOREIGN KEY** → relación con otra tabla  
  - **CHECK** → validar condición  
  - **AUTO_INCREMENT** → ID automático  
  - **DEFAULT** → valor por defecto  
- **ALTER TABLE** → modificar tabla (agregar restricción, cambiar columna)  

## 3. Índices
- **CREATE INDEX** → optimizar búsqueda  
- **DROP INDEX** → eliminar índice  

## 4. Insertar y modificar datos
- **INSERT INTO** → agregar registros  
- **UPDATE** → modificar registros existentes (usar WHERE)  
- **DELETE FROM** → eliminar registros (usar WHERE)  

## 5. Consultas básicas
- **SELECT columnas FROM tabla** → mostrar datos  
- **WHERE** → filtrar registros  
- **ORDER BY** → ordenar resultados  
- **GROUP BY** → agrupar para agregaciones  
- **Funciones de agregación** → SUM(), COUNT(), AVG(), MAX(), MIN()  
- **BETWEEN** → filtrar rango de valores o fechas  

## 6. Índices
- **CREATE INDEX** → mejorar velocidad de consultas  
  *Ejemplo:* `CREATE INDEX idx_usuario_fecha ON Pedidos(usuario_id_fk, fecha_pedido);`  
- **DROP INDEX** → eliminar índice  

## 7. Vistas
- **CREATE VIEW** → guardar una consulta como tabla virtual  
  *Ejemplo:*  
  ```sql
  CREATE VIEW VistaPedidos AS
  SELECT u.correo, p.fecha_pedido, pp.cantidad
  FROM Usuarios u
  INNER JOIN Pedidos p ON u.usuario_id = p.usuario_id
  INNER JOIN PedidoProducto pp ON p.pedido_id = pp.pedido_id;

## 8. JOINs
- **INNER JOIN** → solo registros coincidentes  
- **LEFT JOIN** → todos los de la izquierda + coincidentes de la derecha  
- **RIGHT JOIN** → todos los de la derecha + coincidentes de la izquierda  

## 9. Ejemplo rápido
```sql
-- Crear tabla 
CREATE TABLE Productos (
  producto_id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(50),
  precio DECIMAL(10,2) CHECK(precio > 0)
);

-- Insertar
INSERT INTO Productos(nombre, precio) VALUES('Zapato', 50);

-- Actualizar
UPDATE Productos SET precio = 60 WHERE producto_id = 1;

-- DELETE
DELETE FROM Productos WHERE producto_id = 1;

-- SELECT con JOIN
SELECT u.correo, p.fecha_pedido
FROM Usuarios u
INNER JOIN Pedidos p ON u.usuario_id = p.usuario_id;

-- Agregación y GROUP BY
SELECT pedido_id, SUM(cantidad) AS total_items
FROM PedidoProducto
GROUP BY pedido_id;

-- Crear vista
CREATE VIEW VistaPedidos AS
SELECT u.correo, p.fecha_pedido, pp.cantidad
FROM Usuarios u
INNER JOIN Pedidos p ON u.usuario_id = p.usuario_id
INNER JOIN PedidoProducto pp ON p.pedido_id = pp.pedido_id;
