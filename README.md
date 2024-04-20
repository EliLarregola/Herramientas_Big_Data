### Herramientas_Big_Data
Práctica Integradora de Big Data. 
Se presenta un entorno Docker con Hadoop (HDFS) y la implementación de herramientas como Spark, Hive, HBase, MongoDB, Neo4J, Zeppelin, Kafka, entre otros. 

Se trabaja a través de Putty.

a. Se clona el repositorio:

   ``` git clone https://github.com/lopezdar222/herramientas_big_data ```

b. Se implementa el entorno docker-compose-v1.yml:

   ``` sudo docker-compose -f docker-compose-v1.yml up -d ```


Al ejecutar este comando se utiliza Docker Compose para construir y ejecutar los servicios definidos en el archivo docker-compose-v1.yml en segundo plano, utilizando los privilegios de superusuario. Este archivo YAML define la configuración para un entorno de Big Data, como bases de datos, servidores web, o herramientas de procesamiento de datos, que se ejecutarán como contenedores Docker.

##PASO 1) HDFS

Se configura un entorno de HDFS con Docker, luego se crea una carpeta en el contenedor "namenode" y se copian archivos desde el sistema local a ese contenedor para que estén disponibles para el clúster de HDFS. Se utilizan los siguientes comandos:

```
  sudo docker exec -it namenode bash
  cd home
  mkdir Datasets
  exit
  sudo docker cp <path><archivo> namenode:/home/Datasets/<archivo>
``` 

Para ubicarse en el contenedor namenode utilizo el comando:

 ``` sudo docker exec -it namenode bash ```

Creo un directorio en HDFS llamado "/data":

 ``` hdfs dfs -mkdir -p /data ```

Para corroborar que se creó mi directorio, puedo usar el comando:

  ```  hdfs dfs -ls ```

Se copian los archivos csv provistos a HDFS:

 ``` hdfs dfs -put /home/Datasets/* data ```


Este proceso de creación de la carpeta data y copiado de los archivos, debe poder ejecutarse desde un shell script.

Nota: Busque dfs.blocksize y dfs.replication en <IP_Anfitrion>:9870 para encontrar los valores de tamaño de bloque y factor de réplica respectivamente entre otras configuraciones del sistema Hadoop.

IMAGEN: Paso 1 Archivos en mi HDFS. Asi se muestra luego de entrar por el navegador a <IP_Anfitrion>:9870

##PASO 2) HIVE

El objetivo de este punto, es aprovechar Hive para definir y crear tablas que reflejen la estructura de los datos CSV almacenados en HDFS, facilitando su consulta y análisis posterior dentro del ecosistema de Big Data.
