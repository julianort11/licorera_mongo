# Proyecto: Sistema de Gestión de Licorera

- Jose Julián Ortega  
- Rafael Osorio

## Descripción del sistema

Este sistema es una aplicación para la gestión de una licorera, desarrollado utilizando MongoDB como base de datos no relacional.

El sistema permite:

- Gestionar los productos disponibles (licores, variedades, presentaciones, precios, stock).
- Administrar las diferentes sedes de la licorera.
- Registrar clientes.
- Gestionar empleados.
- Procesar pedidos de los clientes.
- Consultar información mediante búsquedas avanzadas usando expresiones regulares y consultas de impacto.

## Estructura de las colecciones

El sistema está compuesto por 5 colecciones principales:

 #### Extructura Licorera
```Plain Text
taller-mongodb-miapp/
│
├── README.md
├── productos.json
├── sedes.json
├── pedidos.json
├── empleados.json
├── clientes.json
```

### Productos

```json
{
  "_id": 1,
  "nombre": "aguardiente antioqueño",
  "variedad": "tapa azul",
  "provedor": "FLA",
  "stock": 1,
  "presentacion": "1050 ML",
  "porcetajeAlcohol": "29%",
  "precio": 74000,
  "descripcion": "Licor tradicional colombiano"
}
```
### Sedes
```json
{
  "_id": 1,
  "nombre": "Licorera Centro",
  "ciudad": "Bogotá",
  "direccion": "Carrera 8 #12-34",
  "telefono": "3001234567"
}
```
### Pedidos
```json
{
  "id": 1,
  "cliente_id": 1,
  "empleado_id": 1,
  "sede_id": 13,
  "fecha": "2024-11-06",
  "estado": "entregado",
  "total": 150579,
  "productos": [
    {"nombre": "Producto 15", "cantidad": 2, "precio_unitario": 41570, "subtotal": 83140},
    {"nombre": "Producto 11", "cantidad": 1, "precio_unitario": 67439, "subtotal": 67439}
  ]
}
```
### Empleados
```json
{
  "_id": 1,
  "nombre": "aguardiente antioqueño",
  "variedad": "tapa azul",
  "provedor": "FLA",
  "stock": 1,
  "presentacion": "1050 ML",
  "porcetajeAlcohol": "29%",
  "precio": 74000,
  "descripcion": "Licor tradicional colombiano"
}
```
### Clientes
```json
{
  "_id": 1,
  "nombre": "Rafael",
  "cedula": "123456789",
  "fechaDeNacimiento": "1990-05-10",
  "correo": "rafael@gmail.com",
  "direccion": "Calle 666",
  "telefono": "3116261255"
}
```

## Cómo crear la base de datos en MongoDB

### Crear la base de datos
Conectarse a MongoDB (MongoDB Atlas o localmente) y crear la base de datos llamada:

```bash
use licorera
```

### Crear cada colección e insertar los datos
- creamos primero la coleccion con su respectivo nombre (clientes, empleados, pedidos, productos o sedes) en este ejemplo crearemos la coleccion clientes.

![Texto alternativo](/Captura%20de%20pantalla%202025-06-15%20133536.png)

- Acontinuacion despues de crear la coleccion clientes, prodecedemos a hacer el primer paso que es importar el archivo json a compas  

![Texto alternativo](/Captura%20de%20pantalla%202025-06-15%20133602.png)

- despues nos saldra el siguiente cuadro y le daremos importar, y ahora ya tenemos nuestros documentos en las colecciones

![Texto alternativo](/Captura%20de%20pantalla%202025-06-15%20133612.png)

### Consultas sobre la colección cliente
1. Buscar clientes cuya fecha diga que es mayor de edad
```javascript
db.clientes.find({
  fechaDeNacimiento: {
    $regex: "^(19[0-9]{2}|200[0-6]|2007-(0[1-5]|06-(0[1-9]|1[0-5])))"
  }
})
```
******Explicación******: Filtra clientes que tienen al menos 18 años (suponiendo fecha en formato ISO YYYY-MM-DD). Útil para asegurar que solo se atienden clientes legalmente autorizados para comprar licor.

2. Buscar clientes cuyo correo es de Gmail
``` javascript
db.clientes.find({ correo: { $regex: "@gmail\\.com$", $options: "i" } })
```
****Explicación****: Encuentra clientes que usan Gmail, lo que permite personalizar campañas o filtrar clientes por proveedor de correo.
3. Buscar clientes cuya dirección tiene la palabra "Calle"
```javascript
db.clientes.find({ direccion: { $regex: "Calle", $options: "i" } })
```
****Explicación****: Útil para agrupar clientes que residen en zonas urbanas donde las direcciones usan "Calle".
4. Buscar clientes con teléfono que empiece por 311
``` javascript
db.clientes.find({ telefono: { $regex: "^311" } })
```
****Explicación****: Permite identificar clientes con líneas móviles
5. Buscar clientes por parte del nombre
```javascript
db.clientes.find({ nombre: { $regex: "el$", $options: "i" } })
```
****Explicación****: Permite buscar por coincidencia parcial, por ejemplo nombres que terminan en "el" como "Miguel", "Samuel", útil para autocompletar o buscar de forma flexible.

### Consultas sobre la colección empleados
6. Empleados cuyo cargo contiene la palabra "Cajero"
``` javascript
db.empleados.find({ cargo: { $regex: "Cajero", $options: "i" } })
```
****Explicación****: Ayuda a identificar a los empleados con cargos específicos, como cajeros, para filtrarlos en reportes de ventas o turnos.

7. Buscar empleados con correos corporativos
```javascript
db.empleados.find({ correo: { $regex: "@empresa\\.com$", $options: "i" } })
```
****Explicación****: Útil para verificar que los empleados estén registrados con correos institucionales y no personales.

8. Buscar empleados por nombre que contenga "Santiago"
```javascript
db.empleados.find({ nombre: { $regex: "Santiago", $options: "i" } })
```
 ****Explicación****: Filtra empleados con nombres específicos, ideal para búsqueda rápida en interfaz de administración.

9. Buscar empleados nacidos en 1992
```javascript
db.empleados.find({ fechaDeNacimiento: { $regex: "^1992" } })
```
 ****Explicación****: Útil para segmentar empleados por edad o antigüedad aproximada.
10. Buscar empleados cuya dirección contiene "Calle 10"
```javascript
db.empleados.find({ direccion: { $regex: "Calle 10", $options: "i" } })
```
****Explicación****: Permite ubicar empleados en sedes o zonas específicas por su dirección.

### Consultas sobre la colección productos
11. Buscar productos por marca "antioqueño"
```javascript
db.productos.find({ nombre: { $regex: "antioqueño", $options: "i" } })
```
 ****Explicación****: Identifica productos de una marca popular. Útil para stock o análisis de ventas de marcas clave.

12. Buscar presentaciones de 1050 ML
```javascript
db.productos.find({ presentacion: { $regex: "1050", $options: "i" } })
```
**Explicación**: Filtra productos por tamaño o capacidad. Útil para análisis de formatos más vendidos.

13. Buscar productos de proveedor FLA
```javascript
db.productos.find({ provedor: { $regex: "FLA", $options: "i" } })
```
**Explicación**: Ayuda a gestionar relaciones con proveedores y verificar inventario por origen.

14. Buscar productos con porcentaje de alcohol mayor al 20%
```javascript
db.productos.find({ porcetajeAlcohol: { $regex: "2[0-9]%", $options: "i" } })
```
**Explicación**: Útil para clasificar bebidas fuertes, por razones legales, fiscales o de promoción.

15. Buscar productos donde la descripción contiene "a"
```javascript
db.productos.find({ descripcion: { $regex: "a", $options: "i" } })
```
**Explicación**: Busca productos con descripciones que contienen una letra específica, útil para ver si hay textos mal escritos o para pruebas.

### Consultas sobre la colección sedes
16. Buscar sedes de Bogotá
```javascript
db.sedes.find({ ciudad: { $regex: "Bogotá", $options: "i" } })
```
**Explicación**: Permite ver sucursales en una ciudad determinada. Fundamental para reportes o filtros por región.

17. Buscar sedes con nombre que contenga "Centro"
```javascript
db.sedes.find({ nombre: { $regex: "Centro", $options: "i" } })
```
Útil para identificar sucursales ubicadas en zonas centrales.

18. Buscar sedes cuyo teléfono comienza por 300
```javascript
db.sedes.find({ telefono: { $regex: "^300" } })
```
**Explicación**: Filtra sedes con teléfonos de cierto operador o formato, útil para validaciones.

19. Buscar sedes por dirección que tenga "Carrera"
```javascript
db.sedes.find({ direccion: { $regex: "Carrera", $options: "i" } })
```
 **Explicación**: Permite buscar sedes en zonas urbanas comunes de Colombia.

### Consultas sobre la colección pedidos
20. Buscar pedidos entregados
```javascript
db.pedidos.find({ estado: { $regex: "entregado", $options: "i" } })
```
**Explicación**: Útil para hacer seguimiento de pedidos completados y calcular cumplimiento o entregas exitosas.

21. Buscar pedidos realizados en noviembre (mes 11)
```javascript
db.pedidos.find({ fecha: { $regex: "-11-", $options: "i" } })
```
**Explicación**: Permite consultar ventas por mes, por ejemplo para campañas de fin de año.

22. Buscar pedidos con productos que contienen "Producto 15"
```javascript
db.pedidos.find({ "productos.nombre": { $regex: "Producto 15", $options: "i" } })
```
**Explicación**: Verifica en qué pedidos se ha vendido cierto producto, útil para rastreo y control de stock.

23. Buscar pedidos donde haya productos con cantidad 2
```javascript
db.pedidos.find({ "productos.cantidad": { $regex: "^2$" } })
```
**Explicación**: Filtra pedidos con compras repetidas o cantidades comunes para análisis de volumen de venta.

24. Buscar pedidos realizados por cliente_id 1
```javascript
db.pedidos.find({ cliente_id: { $regex: "^1$" } })
```
**Explicación**: Útil para ver historial de compras de un cliente específico.

25. Buscar pedidos que tengan productos cuyo precio unitario termine en 70
```javascript
db.pedidos.find({ "productos.precio_unitario": { $regex: "70$" } })
```
**Explicación**: Identifica productos con precios específicos, útil para validación de promociones o precios estándar.