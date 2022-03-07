# ACCESO

> En shell ->

1.  mongod
2.  mongo

- **`use database_name`**

  > nos situa dentro de la base de datos..., si no está creada, la creará, pero no se registrará la base de datos hasta que se cree una collection

- **`db.createCollection('name');`**

  > crea una colección

- **`db.collection_name.insertOne({campo: value});`**

  > inserta nuestro primer registro

- **`collection.document.find();`**
  > Nos muestra todos los documentos de una collección

> _EJEMPLO: dentro de una collection llamada 'dbtest'
> hay un documento llamado 'datos'...
> db.datos.find('age':23)_

---

## OPERADORES

- **`$eq`** -> equals
- **`$gt`** -> greater than
- **`$lt`** -> less than
- **`$gte`** -> greate than or equal to
- **`$gle`** -> less than or equal to

- **`db.collection.find{llave/campo:{operador:valor_evaluar}}`**
- **_`db.collection.find{'age':{$gt: 12, $lt: 30}}`_**
  > //retorna campos 'age' con valores entre 12 y 30

---

## ALGUNOS COMANDOS

- **`use database_name`**

  > // nos cambia(posiciona) a esa base de datos o en su defecto, la crea

- **`show databases;`**

- **`show dbs;`**

  > // muestra las bases de datos con cuanto pesan

- **`show collections;`**

- **`db.collection.find().pretty();`**

  > //muestra contenido de documentos... .pretty() solo les da formato

- **`db.collection_name.count();`**
  > //devuelve la cantidad de documentos encontrados en una colección
- **`pwd();`**

  > //muestra la dirección a la carpeta a la cual mongodb tiene accesso (esto para cargar archivos desde shell)

  > //Para cambiar el acceso mostrado con pwd... basta con salir de mongo (desde la shell)... entrar a la carpeta deseada (cd carpeta/carpeta/carpeta/archivo.js) y volver a entrar a mongo desde ahi... ahora la pwd nos mostrará el directorio al cual tiene acceso

- **`ls()`**
  > // muestra el listado de ficheros que contiene la carpeta en la que estamos situado dentro de la shell de mondodb
- **`load("archivo_name.js")`**
  > // PERMITE IMPORTAR FICHEROS PARA NUESTRA DB... **TIENEN QUE SER .JS**

---
