# Postgres-Docker

Llevando a cabo la ejecución de un contenedor de la base de datos Postgres. 
Se requiere crear un contenedor el cual disponga de Docker y en el cual sus datos no queden guardados en el contenedor sino en un volumen de docker, para realizar la tarea se crearon tres contenedores distintos de los 
cuáles, dos son de Postgres y uno es de cliente de Postgres con Ubunto 22.04 como base. Primero es necesario crear un volumen (pg_db) en el cual se van a guardar todos los registro fuera de la base de datos y una red que
permita conectar los contenedores entre sí. Luego, crear un contenedor de Postgres, con la imagen postgres:15-bookworm, en el cual se crea un base de datos y se le añade el registro mensaje "Hola mundo" este contenedor 
tiene como volumen pg_db por lo cual los datos persistirán luego de su eliminación en dicho volumen. Posteriormente, se elimina este contenedor y se crea uno nuevo con los mismos parámetros y el mismo volumen. Por ultimo
se crea un contenedor con la imagen ubuntu:20.04 llamado pg_client, en este se instala el cliente Postgres y se conecta a pg_server para verificar si el registro se encuentra lo que demostraría que el volumen esta 
trabajando correctamente.

# Integrantes:

- Juan David Fonseca - 2323942
- Elkin Tovar - 1931440
- Ivan Noriega - 2126012
- Yenny Rivas - 2181527

Link del video: https://youtu.be/D-C3p4DUegI?si=ebBvkJcb7XZltwGh

# Comandos de Docker usados
- Crear la red: docker network create pg_networl
- Crear Volumen: docker volume create pg_db
- Mostrar la redes: docker network ls
- Mostrar volumenes: docker volumen ls
- Crear el contener server primera vez: docker container run --name pg_server --network pg_network -v pg_db:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres -d postgres:15-bookworm
- Ingresar al contenedor pg_server: dokcer container exec -it pg_server /bin/bash
- Dentro del contenegor se crea la base de dato:
  - Ingresar a la base de datos: psql -U postgres
  - Crear la base de datos dentro de la consola de postgres: CREATE DATABASE tarea_db
  - Conectarse a tarea_db : \c tarea_db
  - Crear la tabla: CREATE TABLE pg_tabla ( mensaje VARCHAR(100));
  - Insertar el mensaje detro de la tabla pg_tabla: INSERT INTO pg_tabla VALUES('Hola mundo');
  - Verificar el registro: SELECT * FORM pg_tabla;
- Salir de contenedor: exit
- Eliminar contenedores: Docker rm pg_server -f
- Crear la nueva instancia de pg_server: docker container run --name pg_server --network pg_network -v pg_db:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres -d postgres:15-bookworm
- Crear el contenedor pg_cliente: docker container run --name pg_client --it --network pg_nerwork ubuntu:20.04
  - Instalacion de paquetes necesario: apt update && apt install -y postgresql-client
  - Conexion a pg_server: psql -h pg_server -p 5432 -U postgres tarea_db
  - Verificar el registro guardado en la base de datos gracias al volumen: SELECT * FROM pg_tabla;
- Salir y eliminar todos los contenedores: docker container rm pg_server pg_client -f
  
