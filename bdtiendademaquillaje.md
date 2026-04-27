Aquí tienes un ejemplo claro y práctico para estructurar una base de datos **NoSQL (tipo MongoDB)** para una tienda de maquillaje, incluyendo **base de datos, colecciones, documentos, atributos y tipos de datos**.

---

# 🗄️ Base de datos

**Nombre:** `tienda_maquillaje`

---

# 📁 Colecciones principales

## 1. `productos`

Guarda todos los productos de maquillaje.

### 📄 Documento de ejemplo

```json
{
  "_id": "ObjectId",
  "nombre": "Base líquida Matte",
  "marca": "Maybelline",
  "categoria": "Base",
  "precio": 249.99,
  "stock": 50,
  "tonos_disponibles": ["Claro", "Medio", "Oscuro"],
  "fecha_ingreso": "2026-04-20T10:00:00Z",
  "activo": true
}
```

### 🧾 Atributos y tipos

* `_id`: ObjectId
* `nombre`: String
* `marca`: String
* `categoria`: String
* `precio`: Number (decimal)
* `stock`: Number (entero)
* `tonos_disponibles`: Array de String
* `fecha_ingreso`: Date
* `activo`: Boolean

---

## 2. `clientes`

Información de clientes.

### 📄 Documento de ejemplo

```json
{
  "_id": "ObjectId",
  "nombre": "Ana López",
  "email": "ana@gmail.com",
  "telefono": "6561234567",
  "direccion": {
    "calle": "Av. Principal 123",
    "ciudad": "Ciudad Juárez",
    "codigo_postal": "32000"
  },
  "fecha_registro": "2026-01-15T12:00:00Z"
}
```

### 🧾 Atributos y tipos

* `_id`: ObjectId
* `nombre`: String
* `email`: String
* `telefono`: String
* `direccion`: Object

  * `calle`: String
  * `ciudad`: String
  * `codigo_postal`: String
* `fecha_registro`: Date

---

## 3. `ventas`

Registro de compras.

### 📄 Documento de ejemplo

```json
{
  "_id": "ObjectId",
  "cliente_id": "ObjectId",
  "productos": [
    {
      "producto_id": "ObjectId",
      "nombre": "Labial rojo mate",
      "cantidad": 2,
      "precio_unitario": 150.00
    }
  ],
  "total": 300.00,
  "fecha_venta": "2026-04-25T18:30:00Z",
  "metodo_pago": "Tarjeta"
}
```

### 🧾 Atributos y tipos

* `_id`: ObjectId
* `cliente_id`: ObjectId (referencia)
* `productos`: Array de Object

  * `producto_id`: ObjectId
  * `nombre`: String
  * `cantidad`: Number
  * `precio_unitario`: Number
* `total`: Number
* `fecha_venta`: Date
* `metodo_pago`: String

---

## 4. `proveedores`

Información de proveedores.

### 📄 Documento de ejemplo

```json
{
  "_id": "ObjectId",
  "nombre": "Distribuidora Belleza MX",
  "telefono": "5551234567",
  "email": "ventas@belleza.mx",
  "direccion": "CDMX",
  "productos_suministrados": ["Base líquida Matte", "Corrector"]
}
```

### 🧾 Atributos y tipos

* `_id`: ObjectId
* `nombre`: String
* `telefono`: String
* `email`: String
* `direccion`: String
* `productos_suministrados`: Array de String

---

# 🔗 Relación entre colecciones

* `ventas.cliente_id` → referencia a `clientes._id`
* `ventas.productos.producto_id` → referencia a `productos._id`

---

# 💡 Opcional (mejoras)

Puedes agregar:

* Colección `empleados`
* Colección `categorias`
* Campos como:

  * `descuento`
  * `imagenes`
  * `reseñas`

---

Si quieres, puedo darte el mismo ejemplo pero en **SQL (MySQL o PostgreSQL)** o ayudarte a convertir esto en código listo para usar en MongoDB 🚀
