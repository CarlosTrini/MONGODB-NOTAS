# DOCUMENTOS

## CREAR UN DOCUMENTO

**`use db_name;`**
- **`db.collection_name.insertOne( {campo1: valor1, campo2: valor2...} );`**

## CREAR MULTIPLES DOCUMENTOS

**`use db_name;`**

1.  **`db.collection_name.insertMany( [ {campo1:valor1,campo2:valor2}, 1 {campo1_1:valor1_1, campo2_2:valor2_2} ] );`**

    > en caso de \_id repetido, la ejecución se detiene... y solo inserta aquellos que estan antes de que ocurra el error

2.  **`db.collection_name.insertMany( [ {campo1:valor1,campo2:valor2}, //documento 1 {campo1_1:valor1_1, campo2_2:valor2_2} //documento 2 ], { "oredered": false } );`**
    > // En caso de id repetido, NO detiene la ejecución e inserta todos los documentos... posteriormente se deben eliminar los repetidos
