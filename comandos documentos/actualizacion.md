# ACTUALIZACIONES

## Consideraciones

> **condición: Especifíca los elementos a actulizar**

> **update: Especifica que se va a modificar**

- **`$set`**

  > set toma como argumento un documento

  > actualiza el documento con los campos indicados en `$set` que coincide con el filtro

  > **`{ $set: {campo:valor, campoN:valorN} }`**

---

## Actualizar un único documento

> Actualiza un documento (el primero que encuentre) dentro de una colección basado en algún filtro/condición

- **`db.collection_name.updateOne({condicion},{update},{opciones})`**

  > EJEMPLOS

   1. **`db.collection_name.updateOne({name:'fulano'}, {$set: {edad: 27}});`**

  > Actualizar un campo normal

  2. **`db.collection_name.updateOne({_id: 'some_id'}, {$set: {campo: ['valor1', 'valor2', 'valorN']}});`**

  > Actualizar un vector. Cabe resaltar que esto sustituirá completamente al vector...

  > Notar el uso de "[]" en la expresión: **`{ $set: {campo:['', '', '']} }`**

  3.  **`db.collection_name.updateOne({_id: 'some_id'}, {$set: {'campo.2':'valor'}});`**

  > Actualizar una posición EXACTA de un vector...

  > En caso de que esa posición NO exista, es creada y si dicha posición es más grande que el número consecutivo, los campos intermedios se crean con el valor null... `por ejemplo, se tiene este vector: ['a','b'] el cual tiene la posición 0 y 1... si se inserta $set:{'campo.6':'algo'} el resultado será: ['a','b',null,null,null,null,'algo']`

  > Notar el uso de "[]" en la expresión: **`{ $set: {['', '', '']} }`**

4.
  - **`db.collection_name.updateOne({_id: 'some_id'}, {$set: {'campo.campo_in':'valor'}});`**

    > Actualizar un campo(propiedad) EXACTA de un documento (objeto)...

-  **`db.collection_name.updateOne({_id: 'some_id'}, {$set: {'campo':{campo:valor, campo2:valor2, campoN:valorN}});`**

    > Actualizar TODAS los campos (propiedades). de un documento( objeto)



---

## Actualizar muchos documentos

> Actualiza TODOS los documentos que COINCIDEN con un filtro/condición especificado en una colección

- **`db.collection_name.updateMany({condicion},{update},{opciones})`**

---

## Reemplazar un documento

> Reemplaza un solo documento dentro de una colección que coincide con un filtro/condición

- **`db.collection_name.replaceOne({condicion},{replace},{opciones})`**
