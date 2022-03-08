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

  $mul -> Multiplica el valor de un campo por el valor especificado

    **`{ $mul: {'campo':valor_multiplicador} }`**


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

> Elimina el campo llamado _campo1_ con el valor _valor1_

6. **`db.collection_name.updateOne({_id: 'some_id'}, {$rename: {'campo1': 'nuevo_nombre'});`**

> Renombra el campo llamado _campo1_ con el valor _nuevo_nombre_

7. **`db.collection_name.updateOne({_id: 'some_id'}, {$max: {'year': nuevo_valor});`**

> Suponiendo que el campo _year_ en un inicio tiene el valor de _20_, y se envía esta actualización: _{$max: {'year': 21}}_, el campo _year_ será actualizado con el valor _21_, en caso de ser enviado _{$max: {'year': 19}}_ el campo _year_ NO será actualizado

8. **`db.collection_name.updateOne({_id: 'some_id'}, {$min: {'year': nuevo_valor});`**

> Suponiendo que el campo _year_ en un inicio tiene el valor de _19_, y se envía esta actualización: _{$min: {'year': 18}}_, el campo _year_ será actualizado con el valor _18_, en caso de ser enviado _{$min: {'year': 20}}_ el campo _year_ NO será actualizado

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

> lo que se hace es _{$inc : {'tomato.reviews': 1, 'tomato.stars': 1, year: 5} }_ se están actualizando ambas propiedades del objeto _tomato_ y la propiedad _year_. Notar que al ser _tomato_ un objeto, se usa la notación punto a diferencia de _year_ que no lo es...

> El resultado después de ejecutar lo anterior sería:
> year: 2005 , tomato: { reviews: 2, start: 2 }
> ya que le dijimos que _year_ aumente 5 y que las propiedades de _tomato_ aumente en 1

9. **`db.collection_name.updateOne({title:'pulp'}, {$mul : {'tomato.reviews': 5, 'tomato.stars': 3, year: 2} });`**

> Tomando el documento anterior, _$mul_ lo que hará es tomar el valor especificado y lo usará para multiplicar con el valor del campo actual. Si se toma el documento anterior, los campos se actualizarán a: _tomato:{reviews:5, stars:3} y year:4000_

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

---

## ACTUALIZAR TIPOS VECTORES

## operadores para vectores

```
  Aplican tanto con updateOne y updateMany

  $addToSet -> Agrega elementos a un vector solo si dicho nuevo elemento aún no existen en dicho vector.

    **`{ $addToSet: {vector:valor} }`**

  $push -> Agrega un elemento a un vector, sin importar si este ya existe o no.

    **`{ $push: {'vector':'valor'} }`**

  $pop -> Elimina el primer o último elemento de un vector.
   si se le pasa -1, elimina el primer elemento,
   si se le pasa 1, elimina el último elemento

    **`{ $pop: {'vector': -1 | 1} }`**

  $pull -> Elimina TODOS los elementos de vector que coinciden con una consulta específica por valor o condición.

    **`{ $pull: { campo: valor|condicion} }`**

  $pullAll -> Elimina todos los valores coincidentes de un vector.

    **`{ $pullAll: {'campo':[valor1, valor2, valorN]} }`**

```

## modificadores para vectores

```
  Aplican tanto con updateOne y updateMany

  $each ->
    Modifica los operadores $push y $addToSet para agregar varios elementos para actualizaciones de matrices.

    Se usa con $addToSet para agregar multiples valores a un vector si estos no existen aún. En caso de que un valor ya exista, este no será repetido

    **`{$addToSet: { vector: { $each: [ valor1, valorN ] } } }
`**
    Se usa con $push para agregar multiples valores a un vector existan o no. Puede provocar que se repitan elementos

    **`{$push: { vector: { $each: [ valor1, valorN ] } } }
`**


  $position -> Modifica al operador $push, para indicarle la posición en la que debe insertar el nuevo valor. Es necesario usar el modificador $each

    **`{
         $push: {
          campo: {
            $each: [ valor1, valor2, valorN, ... ],
            $position: número_de_posición
          }
        }
      }}`**

  $slice ->

    **`{ $slice: {'campo':nuevo_valor_numérico} }`**

  $sort ->

    **`{ $sort: {'campo':nuevo_valor_numérico} }`**

```

### --------EJEMPLOS OPERADORES

1.  **`db.collection_name.updateMany({movie: true}, {$set:{'audiencia':['infantil','jovenes','adulto']}});`**

> Actualiza los documentos que contienen el campo _'movie'_ como _true_... asignandoles el campo _'audiencia'_ con el vector _['infantil','jovenes','adulto']_

2.  **`db.collection_name.updateMany({movie: true}, {$set:{'audiencia.1':'youngs'}});`**

> Actualiza los documentos que contienen el campo _'movie'_ como _true_... modificando la _posición 1_ de manera especifica _'audiencia.1'_ con el valor _youngs_

3.  **`db.collection_name.updateMany({campo: valor}, {$addToSet:{'vector':'nuevo_valor'}});`**

> Si el nuevo valor en _$addToSet_ no existe en el vector _vector_ entonces se agrega, de lo contrario, el vector _vector_ no sufre ningún cambio

4.  **`db.collection_name.updateMany({campo: valor}, {$push:{'vector':'nuevo_valor'}});`**

> _push_ agrega un nuevo al campo _vector_ exista el valor o no. Causando esto valores duplicados si ya se contiene dicho valor. para evitar esto usar _$addToSet_

5.  **`db.collection_name.updateMany({campo: valor}, {$pop:{'vector': -1 | 1}});`**

> _pop_ usando _{$pop: {vector: -1}}_ se eliminará el primer elemento del campo _vector_.

> usando _{$pop: {vector: 1}}_ se eliminará el último elemento del campo _vector_

5.  **`db.collection_name.updateMany({campo: valor}, { $pull: { vector: 'valor' | condicion }} )`**

> _pull_ usando _{$pull: {vector: 'valor_buscado'}}_ se eliminará el primer elemento del campo _vector_ que coincida con el valor dado _valor_buscado_.

> usando _{$pull: {vector: {$gt : 5} }}_ se eliminarán aquellos números mayores a 5

6.  **`db.collection_name.updateMany({campo: valor}, { $pullAll: { vector: [valor, valorN...] }} )`**

> A diferencia del operador $*pull* que elimina elementos especificando una consulta, *$pullAll* elimina elementos que coinciden con los valores enumerados.*

> suponiendo que tenemos un vector como este: scores: [ 0, 2, 5, 5, 1, 0 ]

> _pullAll_ usando _{$pullAll: {scores: [0, 5]}}_ eliminará todos aquellos valores que coinciden con los valores enviados en _[0, 5]_ por lo que esto aplicado al vector de ejemplo, lo actualizaría a _scores: [ 2, 1 ]_

### --------EJEMPLOS MODIFICADORES

1.  **`db.collection_name.updateMany({movie: true}, {$addToSet:{'vector':{$each: [valor, valorN, ...]}}});`**

> _$each con $addToSet y $push_.

> Suponiendo que tenemos el campo: _numeros:[1,2,3,4]_

> si se usa el siguiente query: _{$addToSet : { numeros : {$each: [4,5,6,7,8] }} }_ -> El resultado sería : _numeros: [1,2,3,4,5,6,7,8]_ Como se observa al usar _$addToSet_ el valor _4_ no se repite.

> si se usa el siguiente query: _{ $push: { numeros: {$each: [4,5,6,7,8]} } }_ -> El resultado sería: _numeros:[1,2,3,4,4,5,6,7,8]_ Como se observa, al usar _$push_ el valor _4_ si se repite.

2.  **`db.collection_name.updateMany({movie: true}, {$push:{'vector':{$each: [valor, valorN, ...], $position: numero_posicion } } });`**

> *$position* modificador para *$push* hace uso de *$each*...

> Suponiendo que tenemos el campo: *vocales*:['a', 'e']_ el cual tiene dos posiciones *0 y 1*

> si se usa el siguiente query: _{$push : { vocales : {$each: ['i', 'o', 'u'], $position: 2 }} }_ -> El resultado sería : _vocales: ['a', 'e','i', 'o', 'u' ]_ Como se observa  _$each y $position_ están dentro de *vocales: {$each : [] , $position: 3}* separados por una coma


---

## ACTUALIZAR MUCHOS DOCUMENTOS (updateMany)

### Aqui se muestran ejemplos que funcionan tanto para vectores como documentos

> Actualiza TODOS los documentos que COINCIDEN con un filtro/condición especificado en una colección

- **`db.collection_name.updateMany({condicion},{update},{opciones})`**

  1.  **`db.collection_name.updateMany({}, {$set: {nss: false}});`**

  > Actualiza todos los documentos, asignandoles el campo _'nss'_ en _false_

---

---

## Tanto en vectores como en documentos, se puede usar updateMany o updateOne, solo que hay que tener cuidado al momento de actualizar un documento(objeto) o vector, ya que si no se indica la posición deseada, todo el campo será reemplazado, perdiendo los valores anteriores...
