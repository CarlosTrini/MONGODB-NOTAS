# ACTUALIZACIONES

## Consideraciones

> **condición: Especifíca los elementos a actulizar**

> **update: Especifica que se va a modificar**

- **`$set`**

  > set toma como argumento un documento

  > actualiza el documento con los campos indicados en `$set` que coincide con el filtro

  > **`{ $set: {campo:valor, campoN:valorN} }`**
---
  ---
  ---
  ---

## ACTUALIZAR UN DOCUMENTO (updateOne)


## operadores para campos

```
  Aplican tanto con updateOne y updateMany

  $unset -> Elimina el campo indicado
    
    **`{ $unset: {campo:valor} }`**

  $rename -> Renombra el campo indicado

    **`{ $rename: {'nombre_actual':'nombre_nuevo'} }`**

  $max -> Actualiza si el nuevo valor especificado ES MAYOR al valor actual en el campo seleccionado

    **`{ $max: {'campo':nuevo_valor_numérico} }`**

  $min -> Actualiza si el nuevo valor especificado ES MENOR la valor actual en el campo seleccionado

    **`{ $min: {'campo':nuevo_valor_numérico} }`**

  $inc -> Incrementa el valor del campo en base al valor especificado

    **`{ $inc: {'campo':aumento} }`**


```

> Actualiza un documento (el primero que encuentre) dentro de una colección basado en algún filtro/condición

- **`db.collection_name.updateOne({condicion},{update},{opciones})`**

  
  ### EJEMPLOS

  1.  **`db.collection_name.updateOne({name:'fulano'}, {$set: {edad: 27}});`**

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

- **`db.collection_name.updateOne({_id: 'some_id'}, {$set: {'campo':{campo:valor, campo2:valor2, campoN:valorN}});`**

  > Actualizar TODAS los campos (propiedades). de un documento( objeto)

5. **`db.collection_name.updateOne({_id: 'some_id'}, {$unset: {'campo1': valor1});`**

  > Elimina el campo llamado *campo1* con el valor *valor1*


6. **`db.collection_name.updateOne({_id: 'some_id'}, {$rename: {'campo1': 'nuevo_nombre'});`**

  > Renombra el campo llamado *campo1* con el valor *nuevo_nombre*


7. **`db.collection_name.updateOne({_id: 'some_id'}, {$max: {'year': nuevo_valor});`**

  > Suponiendo que el campo *year* en un inicio tiene el valor de *20*, y se envía esta actualización: *{$max: {'year': 21}}*, el campo *year* será actualizado con el valor *21*, en caso de ser enviado  *{$max: {'year': 19}}* el campo *year* NO será actualizado

8. **`db.collection_name.updateOne({_id: 'some_id'}, {$min: {'year': nuevo_valor});`**

  > Suponiendo que el campo *year* en un inicio tiene el valor de *19*, y se envía esta actualización: *{$min: {'year': 18}}*, el campo *year* será actualizado con el valor *18*, en caso de  ser enviado  *{$min: {'year': 20}}* el campo *year* NO será actualizado

8. **`db.collection_name.updateOne({title:'pulp'}, {$inc : {'tomato.reviews': 1, 'tomato.stars': 1, year: 5} });`**

 > Para este ejemplo se usa en siguiente documento

 ```
  {
    "_id": {
        "$oid": "622415a9b9d52c960ba6a9ae"
    },
    "title": "pulp",
    "year": 2000,
    "tomato": {
        "reviews": 1,
        "stars": 1
    }
}
 ```

 > lo que se hace es *{$inc : {'tomato.reviews': 1, 'tomato.stars': 1, year: 5} }* se están actualizando ambas propiedades del objeto *tomato* y la propiedad *year*. Notar que al ser *tomato* un objeto, se usa la notación punto a diferencia de *year* que no lo es...

 > El resultado después de ejecutar lo anterior sería: 
 > year: 2005 , tomato: { reviews: 2, start: 2 }
 > ya que le dijimos que *year* aumente 5 y que las propiedades de *tomato* aumente en 1



---

## ACTUALIZAR MUCHOS DOCUMENTOS (updateMany)

### Aqui se muestran ejemplos que funcionan tanto para vectores como documentos

> Actualiza TODOS los documentos que COINCIDEN con un filtro/condición especificado en una colección

- **`db.collection_name.updateMany({condicion},{update},{opciones})`**

  1.  **`db.collection_name.updateMany({}, {$set: {nss: false}});`**

  > Actualiza todos los documentos, asignandoles el campo _'nss'_ en _false_

---

## ACTUALIZAR TIPOS DOCUMENTO (OBJETO)

```

```

> Reemplaza un solo documento dentro de una colección que coincide con un filtro/condición

- **`db.collection_name.replaceOne({condicion},{replace},{opciones})`**

  1.  **`db.collection_name.updateOne({'id':'123'}, {$set: {'base.plat': 'YOUTUBE'}} );`**

  > Notar que se hace uso de la notación con punto

  > Actualiza el documentos que contienen el campo _'id'_ con valor _123... Reemplaza el valor de esa posición con \_YOUTUBE_ capitalizado

  2.  **`db.collection_name.updateMany({base:'youtube'}, {$set: {base: {plat:'youtube', chan:'pelisYout'}}} );`**

  > Actualiza los documentos que contienen el campo _'base'_ con valor _youtube_... Reemplaza ese campo de tipo string, con uno de tipo _vector_ que es {_plat:'youtube', chan:'pelisYout'}_

  3.  **`db.collection_name.updateMany({'base.plat':'youtube'}, {$set: {'base.plat': 'YOUTUBE'}} );`**

  > Notar que se hace uso de la notación con punto

  > Actualiza los documentos que contienen el campo _'base.plat'_ con valor _youtube_... Reemplaza el valor de esa posición con _YOUTUBE_ capitalizado

 ## operadores

  - **`$unset`**
  
    1. **`db.collection_name.updateOne({title:'el malo'}, {$unset:{year: 2010}})`**
    
    > Elimina el campo year del documento

---

## ACTUALIZAR TIPOS VECTORES

## operadores para campos

```
  Aplican tanto con updateOne y updateMany

  $addToSet ->
    
    **`{ $unset: {campo:valor} }`**

  $push ->

    **`{ $rename: {'nombre_actual':'nombre_nuevo'} }`**

  $pop -> 

    **`{ $max: {'campo':nuevo_valor_numérico} }`**

  $pull ->

    **`{ $min: {'campo':nuevo_valor_numérico} }`**

  $pullAll ->

    **`{ $inc: {'campo':aumento} }`**


```

 1.  **`db.collection_name.updateMany({movie: true}, {$set:{'audiencia':['infantil','jovenes','adulto']}});`**

 > Actualiza los documentos que contienen el campo _'movie'_ como _true_... asignandoles el campo _'audiencia'_ con el vector _['infantil','jovenes','adulto']_

 2.  **`db.collection_name.updateMany({movie: true}, {$set:{'audiencia.1':'youngs'}});`**

  > Actualiza los documentos que contienen el campo _'movie'_ como _true_... modificando la _posición 1_ de manera especifica _'audiencia.1'_ con el valor _youngs_
---

## Tanto en vectores como en documentos, se puede usar updateMany o updateOne, solo que hay que tener cuidado al momento de actualizar un documento(objeto) o vector, ya que si no se indica la posición deseada, todo el campo será reemplazado, perdiendo los valores anteriores...