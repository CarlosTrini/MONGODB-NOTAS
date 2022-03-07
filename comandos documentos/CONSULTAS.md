# MOSTRAR DOCUMENTOS DE UNA COLECCIÓN( LECTURAS )

---

## CONSULTAS GENERALES

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

## CONSULTAS A CAMPOS TIPO "DOCUMENTO" (OBJETOS {})

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

## CONSULTAS A CAMPOS TIPO "VECTOR" (ARRAYS [])

**ejemplo... tenemos este documento->**

```
{
  "\_id": { $oid": "622423ddc8420c43fe8cec1e" },
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
