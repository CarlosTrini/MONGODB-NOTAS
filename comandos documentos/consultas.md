# MOSTRAR DOCUMENTOS DE UNA COLECCIÓN( LECTURAS )

---

## **CONSULTAS GENERALES**

`use.db_name;`

- **`db.collection_name.find()`;**

  > // retorna todos los documentos de la colección

- **`db.collection_name.find();`**

  > // retorna valor numérico de los documentos de lcolección\_\_

- **`db.collection_name.find().pretty();`**

  > //pretty() da formato visual

- **`db.collection_name.find({campo: value});`**

  > //muestra campos que coinciden con campo:value...

- **`db.collection_name.find({campo: value}).count();`**
  > //retorna la cantidad numérica de documentos hayados
- **`db.collection_name.find( {campo1:value, campo2:value, campoN:valueN} );`**
  > //si se usan dos o mas campos, es como usar el operador 'and' {campo1: value y campo2:value}

---

## **CONSULTAS A CAMPOS TIPO "DOCUMENTO" (OBJETOS {})**

**ejemplo... tenemos este documento->**

```
{
    nombre: 'fulano',
    edad: 28,
    datosEscolares : {
        id:"abcd",
        grado: "tercero",
    }
}
```

`use.db_name;`

- **`db.collection_name.find( { nombre:fulano, "datosEscolares.id":"abcd" } );`**
  > 1. retorna al que coincida con el nombre y el id de datosEscolares...
  > 2. para buscar el campos de tipo DOCUMENTO se usa la notación de PUNTO
  > 3. es IMPORTANTE el uso de comillas al usar notación de punto como en : "datosEscolares.id"

---

## **CONSULTAS A CAMPOS TIPO "VECTOR" (ARRAYS [])**

**ejemplo... tenemos este documento->**

```
{
  "_id": { $oid": "622423ddc8420c43fe8cec1e" },
  "title": "No Escape",
  "director": "John Erick Dowdle",
  "writers": ["John Erick Dowdle", "Drew Dowdle"],
  "actors": ["Owen Wilson", "Lake Bell", "Sterling Jerins", "Claire Geare"],
}
```

> **hay formas de acceder a los datos de un VECTOR**

1. Que contenga lo indicado, SIN IMPORTAR LA POSICIÓN
   `use.db_name;`

   - **`db.collection_name.find( {"actors":"Lake Bell"} );`**
     > //si lo contiene, lo retorna

2. Que contenga el vector **EXACTAMENTE** como se indica (que el vector sea IGUAL) buscamos un vector **EN EL MISMO ORDEN Y CON LOS MISMO ELEMENTOS**

`use.db_name;`

- **`db.collection_name.find( {"writers": ["John Erick Dowdle", "Drew Dowdle"] } );`**

- > NOTA 1: **Se hace uso de corchetes...**
- > NOTA 2: el arreglo es EXACTAMENTE igual al del documento, EN CASO de que algún valor fuera diferente tanto en valor como el posición, no encontraría a este documento

3. Que contenga el elemento en la **posición exacta indicada... SE UTILIZA NOTACION DE PUNTO (.)**

`use.db_name;`

- **`db.collection_name.find( {"writers.0": "John Erick Dowdle"} );`**

- > NOTA 1: en el documento de ejemplo, "John Erick Dowdle" se encuentra en la posición 0
- > NOTA 2: **ES NECESARIO el uso de las comillas el el campo a evaluar al indicar la posición "campo.posiciónNumérica"**
  > En caso de que el vector SI contenga el valor PERO LA POSICIÓN NO sea la indicada, NO MOSTRARÁ NADA
- > NOTA 3: Si el vector contiene más de un valor pero y SI coincide el valor que se le indica en la posición que se le indica, LO RETORNA... Siempre y cuando este en la posición y sea el mismo valor

---

## **CURSOR**

> Un cursor es un puntero que **nos mantiene una referencia sobre la consulta realizada...** es decir, si se hace una consulta a una collection que contiene 100 documentos y le pedimos a mongo que nos muestre de 20 en 20, la primera vez nos mostrará del 0 - 20, el cursor guarda la referencia a los otros 80 documentos restantes... en consola, para poder seguir viendo lo que mantiene el cursor se usa la palabra **"it"**. Una vez que la consulta muestre los 100 documentos, el cursor ha terminado

> Las consultas **"find"** retornan un cursor

---

## **PROYECCIÓN**

> Una proyección permite especificar que campos se quieren mostrar de una consulta.

> **Si se tiene una consulta que retorna: _id_, _nombre_, _direccion_, _estatus_ pero solo se desea extraer el nombre, o estatus o ambas en lugar de TODOS LOS CAMPOS** se puede hacer uso de las proyecciones

- **`db.collection_name.find( {edad: {$gt:18}}, {nombre: 1, edad:1} );`**

  > _**es como decir: Traeme solo el nombre y la edad de aquellos documentos cuya propiedad edad sea mayor que 18**_

  1. Query: `{edad: {$gt:18}}`
  2. Proyección:`{nombre:1, edad:1}`
     > // query es la condición de consulta

     > // solo nos mostrará la edad y el nombre;

     > // **el valor 1 o true**, es para indicar que queremos que nos muestre ese valor, en caso de no querer más, solo se omiten

     > // **el valor 0 o false**, es para indicar que se excluye un campo de los resultados

- **`db.collection_name.find( {}, {nombre: 1} );`**

  > _**es como decir: Traeme solo el nombre de TODOS los documentos que tengas en esta colección**_

  1. Query: `{}`
  2. Proyección:`{nombre:1}`

     > // en este caso el **QUERY SE ENCUENTRA VACÍO** pero ponerlo es importante, ya que nuestra proyección debe estar después de las llaves de nuestro query

     > // Al colocar el query vacío quiere decir que la proyección la aplique a todos los campos y no los muestre.

- **`db.collection_name.find( {}, {nombre: 0} );`**

  > _**es como decir: Traeme TODOS los documentos que tengas en esta colección y MUESTRAME todos sus campos de c/u, EXCEPTO el campo nombre y el campo id**_

  1. Query: `{}`
  2. Proyección:`{nombre:0, _id:0}`

     > // Ya vimos que el query puede estár vacío o contener alguna condición

     > // AL colocar el 0 en algún campo, este no se mostrará en los resultado de la consulta

---

## **LIMITE**
 > El métod **.limit()** nos permite definir la cantidad de registros que queremos que se nos retorne en una consulta

- **`db.collection_name.find().limit(5);`**
- **`db.collection_name.find( {SOME_CONDITION}).limit(5);`**
 - **`db.collection_name.find( {SOME_CONDITION}, {SOME_PROYECTION} ).limit(5);`**

     > // En cualquiera de los ejemplos nos retornará 5 documentos


