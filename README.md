# Proyecto Final - Diseño y Arquitectura de Software

## Integrantes:
- Francisco Alan Menchaca Merino - [@alanmenchaca](https://github.com/alanmenchaca)
- Adrian Led Vazquez Herrera - [@LedVazquez](https://github.com/LedVazquez)
- Jesús Raúl Alvarado Torres - [@RaulAlvaradoT](https://github.com/RaulAlvaradoT)

## Herramientas utilizadas:
- [Docker](https://www.docker.com)
  - Docker Files
  - Docker Compose  
- [Python](https://www.python.org)
  - PyMongo
  - Numpy
  - Faker
  - Flask
- [Mongodb](https://www.mongodb.com)
- [Mongo-express](https://hub.docker.com/_/mongo-express)

## Diagrama de Microservicios

## Clases utilizadas en nuestro proyecto:

### MODULO: user_transaction 
   * `Transaction`: El objetivo de esta clase es poder crear instancias que simulen una transacción.
#### ------ **ATRIBUTOS** ------
   * `referencia`: Número de 6 dígitos que se incrementa de manera automática al crear una instancia de la clase Transaction.
   * `date`: Fecha de cuando se realizó la transacción (se genera de manera automática utilizando la librería Faker).
   * `amount`: Monto de la transacción (puede ser positivo o negativo).
   * `type`: Representa el tipo de transacción (inflow o outflow), que se genera de acuerdo al monto (si el monto es mayor o igual a cero, el tipo de transacción es inflow, de lo contrario el tipo de la transacción es outflow).
   * `category`: La categoría de la transacción (salary, savings, groceries, transfer, rent, other).
   * `user`: El nombre de la persona de la transacción.
   * `user_email`: El e-mail de la persona de la transacción.
#### ------ **FUNCIONES** ------
   * `set_user_data`: Se utiliza para asignar el nombre y el e-mail de la persona de la transacción, los cuales se obtienen por parámetros.
   * `_get_object_to_dict`: Retorna el objeto creado en forma de diccionario.
   * `TransactionGenerator`: El objetivo de esta clase es poder crear una lista de Transaciones generadas de manera aleatoria.
   * `generate_rnd_transactions`: Esta función crea transacciones generadas de manera aleatoria, esto de acuerdo al número de usuarios ingresado por parámetros, e.g. por cada usuario se genera una transacción de categoría: salary, savings, groceries, transfer, rent y other, de modo que por cada usuario se generan 6 transacciones (si el número de usuarios es igual a 2, se generan 12 transacciones).
   
### MODULO: Mongo_db 
* `UserTransactionDB`: Esta clase se utiliza para crear una base de datos en Mongodb.
#### ------ **ATRIBUTOS** ------
   * `client`: Cliente necesario para crear una base de datos en MongoDB o para crear colecciones de documentos en MongoDB.
   * `users_db`: Atributo que contiene la base de datos de MongoDB
   * `transactions`: Atributo que contiene una colección de la base de datos de MongoDB
#### ------ **FUNCIONES** ------
   * `_generate_mongo_client`: Genera una instancia de tipo MongoClient.
         *  Host: mongo_db
         *  port: 27017
         *  username: root
         *  password: kberl
   * `create_mongo_db`: Crea una base de datos en MongoDB, llamada "users_db" y una colección llamada: "transactions", por defecto.
   * `get_db_collection`: Retorna la colección ya creada (la colección "transactions" en este caso).

Con las clases anteriormente descritas e implementadas en código es posible tomar en cuenta los siguientes puntos:
* La `referencia` de una transacción es única
* Solamente existen dos tipos de transacción: `inflow` y `outflow`
* Todas las transacciones de tipo `outflow` son numeros decimales negativos
* Todas las transacciones de tipo `inflow` son numeros decimales positivos
* Es posible recibir transacciones en masa también, ya sea por medio de un archivo externo o dentro de una misma petición

Con ayuda de un documento de tipo `docker-compose.yml` podemos crear imagenes e inicializar containers de: mongo_db, mongo-express y flask, con el contenedor de mongo_db podemos crear la base de datos de las de las transacciones de los usuarios y una ves habiendo generado las transacciones de manera aleatoria, podemos crear una colección que contenga todas las transacciones. 

![image](https://user-images.githubusercontent.com/71090472/173277538-3b2c4c1c-47ba-44a0-8da3-1c6b2a305fb2.png)

Utilizando mongo-express en el host:localhost, puerto:8081, es posible visualizar la base de datos posteriormente creada y llenada:

![image](https://user-images.githubusercontent.com/71090472/173277625-e4e0d63b-7306-4255-a75d-1b020718d301.png)

![image](https://user-images.githubusercontent.com/71090472/173277663-c8c3c36d-5162-4130-afe1-7b48411b2adf.png)

Otra manera de poder visualizar las transacciones pero esta ves de manera renderizada en HTML en formato JSON, es con el uso del container de Flask en el host: http://192.168.1.71/, puerto: 5000, endpoint: transactions (contenedor creado en nuestro docker-compose)

![image](https://user-images.githubusercontent.com/71090472/173273721-45946101-aae4-4d61-bff0-0aa931a5050e.png)

* En el endpoint: transactions/grouped_by_type con el método: GET somos capaces de ver un resumen que nos muestra el `inflow` y `outflow` total por usuario. 
  * GET /transactions/grouped_by_type

![image](https://user-images.githubusercontent.com/71090472/173278274-ee1a9487-0c6a-43e9-afc5-6737535aee2e.png)

* En el endpoint: transactions/Eric Powell/summary podemos ver un resumen del usuario (Eric Powell) por categoría en formato JSON, este endpoint muestra la suma de cantidades por categoría de transacción.
  * GET /transactions/Eric Powell/summary

![image](https://user-images.githubusercontent.com/71090472/173277988-e11d29ae-e5b1-425f-8871-5c404cc7ef24.png)

## Documentación:
- [Docker](https://docs.docker.com)
- [Python](https://docs.python.org/3)
- [Mongodb](https://www.mongodb.com/docs)
- [Flask](https://flask.palletsprojects.com/en/2.1.x)
- [Markdown](https://www.markdownguide.org/basic-syntax)
