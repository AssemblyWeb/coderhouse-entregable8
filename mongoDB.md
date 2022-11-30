> Consigna: Utilizando Mongo Shell, crear una base de datos llamada ecommerce que contenga dos colecciones: mensajes y productos.

1. Agregar 10 documentos con valores distintos a las colecciones mensajes y productos. El formato de los documentos debe estar en correspondencia con el que venimos utilizando en el entregable con base de datos MariaDB.
2. Definir las claves de los documentos en relación a los campos de las tablas de esa base. En el caso de los productos, poner valores al campo precio entre los 100 y 5000 pesos(eligiendo valores intermedios, ej: 120, 580, 900, 1280, 1700, 2300, 2860, 3350, 4320, 4990).

```
use coderhouse_ecommerce

const mensajes = [
    {
        email: "alfio@mail.com",
        message: "Hola",
        date: "29/11/22 16:50:03"
    },
    {
        email: "nacho@mgail.com",
        message: "Buenos días",
        date: "29/11/22 16:51:03"
    },
    {
        email: "carlos@gmail.com",
        message: "Buenas",
        date: "29/11/22 16:50:10"
    },
    {
        email: "antonio@gmail.com",
        message: "hola",
        date: "29/11/22 16:53:03"
    },
    {
        email: "gabriel@gmail.com",
        message: "hello",
        date: "29/11/22 16:55:33"
    },
    {
        email: "gmail@gmail.com",
        message: "hi",
        date: "29/11/22 16:58:03"
    },
    {
        email: "alfio@gmail.com",
        message: "buenos días",
        date: "29/11/22 16:58:10"
    },
    {
        email: "alfio@gmail.com",
        message: "buenas",
        date: "29/11/22 16:58:45"
    },
    {
        email: "alfio@gmail.com",
        message: "qué tal?",
        date: "29/11/22 16:59:03"
    },
    {
        email: "dikego@gmail.com",
        message: "Hola, qué tal?",
        date: "29/11/22 17:00:03"
    }
]

const productos = [
    {
        title: "Casa",
        price: 100,
        thumbnail: "/casa.png"
    },
    {
        title: "avionn",
        price: 900,
        thumbnail: "/avion.png"
    },
    {
        title: "manzana",
        price: 1550,
        thumbnail: "/green.png"
    },
    {
        title: "Tanque,
        price: 670,
        thumbnail: "tanque.png"
    },
    {
        title: "Remera",
        price: 4578,
        thumbnail: "/reme.png"
    },
    {
        title: "lancha",
        price: 5000,
        thumbnail: "/lanchan.png"
    },
    {
        title: "moto",
        price: 120,
        thumbnail: "motoneta.png"
    },
    {
        title: "bici",
        price: 4990,
        thumbnail: "bici.png"
    },
    {
        title: "Patin",
        price: 2354,
        thumbnail: "pating.svg"
    },
    {
        title: "roller",
        price: 1453,
        thumbnail: "/roller.png"
    }
]

db.mensajes.insertMany(mensajes)

db.productos.insertMany(productos)
```

3. Listar todos los documentos en cada colección.

```
db.productos.find()
sb.mensajes.find()
```

4. Mostrar la cantidad de documentos almacenados en cada una de ellas.

```
db.productos.estimatedDocumentCount()
db.mensajes.estimatedDocumentCount()
```

5. Realizar un CRUD sobre la colección de productos:

- Agregar un producto más en la colección de productos

```
db.productos.insertOne({nombre: "Guitarra", precio: 1499, thumbnail: "guitar.png"})
```

- Realizar una consulta por nombre de producto específico:

  - Listar los productos con precio menor a 1000 pesos.

    ```
    db.productos.find({ price: { $lt: 1000 } })
    ```

  - Listar los productos con precio entre los 1000 a 3000 pesos.

    ```
    db.productos.find({ $and: [{ price: { $gt: 1000 } }, { price: { $lt: 3000 } }] })

    ```

  - Listar los productos con precio mayor a 3000 pesos.
    ```
    db.productos.find( { price: { $gt: 3000} } )
    ```
  - Realizar una consulta que traiga sólo el nombre del tercer producto más barato.
    ```
    db.productos.find({ },{ title: 1, _id: 0 }).sort({ order: 1}).skip({ 2 }).limit({ 1 })
    ```
  - Hacer una actualización sobre todos los productos, agregando el campo stock a todos ellos con un valor de 100.
    ```
    db.productos.updateMany( {}, { $set: { stock: 100} } )
    ```
  - Cambiar el stock a cero de los productos con precios mayores a 4000 pesos.
    ```
    db.productos.updateMany({ price: {$gt: 4000} }, { $set: { stock: 0 } })
    ```
  - Borrar los productos con precio menor a 1000 pesos
    ```
    db.productos.deleteMany({ price: {$lt: 1000} })
    ```

6. Crear un usuario 'pepe' clave: 'asd456' que sólo pueda leer la base de datos ecommerce. Verificar que pepe no pueda cambiar la información.

```
use admin
db.createUser({
    user: "pepe",
    pwd: "asd456",
    roles: [{ role: "read", db: "ecommerce" }]
})
```
