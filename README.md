<h1><strong>HERRAMIENTAS DEL BIG DATA</strong></h1>

Práctica Integradora de Big Data. 
Se presenta un entorno Docker con Hadoop (HDFS) y la implementación de herramientas como Spark, Hive, HBase, MongoDB, Neo4J, Zeppelin, Kafka, entre otros. 

![image](https://github.com/EliLarregola/Herramientas_Big_Data/assets/91983204/c4fd0e0b-3f4a-49e0-a6d0-2e4f5a69f3cc)

Se trabaja a través de Putty.

a. Se clona el repositorio:

   ``` git clone https://github.com/lopezdar222/herramientas_big_data ```

b. Se implementa el entorno docker-compose-v1.yml:

   ``` sudo docker-compose -f docker-compose-v1.yml up -d ```


Al ejecutar este comando se utiliza Docker Compose para construir y ejecutar los servicios definidos en el archivo docker-compose-v1.yml en segundo plano, utilizando los privilegios de superusuario. Este archivo YAML define la configuración para un entorno de Big Data, como bases de datos, servidores web, o herramientas de procesamiento de datos, que se ejecutarán como contenedores Docker.

## PASO 1) HDFS

Se configura un entorno de HDFS con Docker, luego se crea una carpeta en el contenedor "namenode" y se copian archivos desde el sistema local a ese contenedor para que estén disponibles para el clúster de HDFS. Se utilizan los siguientes comandos:

```
  sudo docker exec -it namenode bash
  cd home
  mkdir Datasets
  exit
  sudo docker cp <path><archivo> namenode:/home/Datasets/<archivo>

sudo docker cp Datasets/canaldeventa/CanalDeVenta.csv namenode:/home/Datasets/canaldeventa/CanalDeVenta.csv

``` 

Para ubicarse en el contenedor namenode utilizo el comando:

 ``` sudo docker exec -it namenode bash ```

Creo un directorio en HDFS llamado "/data":

 ``` hdfs dfs -mkdir -p /data ```

Para corroborar que se creó mi directorio, puedo usar el comando:

  ```  hdfs dfs -ls ```

Se copian los archivos csv provistos a HDFS:

 ``` hdfs dfs -put /home/Datasets/* /data ```


Este proceso de creación de la carpeta data y copiado de los archivos, debe poder ejecutarse desde un shell script.

Nota: Busque dfs.blocksize y dfs.replication en <IP_Anfitrion>:9870 para encontrar los valores de tamaño de bloque y factor de réplica respectivamente (en este caso 3) entre otras configuraciones del sistema Hadoop.

![Paso 1 Archivos en mi HDFS](https://github.com/EliLarregola/Herramientas_Big_Data/assets/91983204/6e13e7ab-b1cd-4401-a1af-cf5e7adbfc10)
*Paso 1 Archivos en mi HDFS. Asi se muestra luego de entrar por el navegador a <IP_Anfitrion>:9870*

## PASO 2) HIVE

El objetivo de este punto, es aprovechar Hive para definir y crear tablas que reflejen la estructura de los datos CSV almacenados en HDFS, facilitando su consulta y análisis posterior dentro del ecosistema de Big Data.

IMAGEN HIVE

En este caso se utiliza el entorno docker-compose-v2.yml. Con el siguiente comando iniciamos un entorno con Hive:

``` sudo docker-compose -f docker-compose-v2.yml up -d ```

Es importante asegurarse en que parte del directorio estamos ubicados a la hora de ejecutar el comando anterior.

![image](https://github.com/EliLarregola/Herramientas_Big_Data/assets/91983204/7c871eaf-0cb1-4ecf-8ed8-27963a6062a8)


Para crear tablas en Hive a partir de los csv ingestados en HDFS, en primer lugar se pasa el archivo Paso02.hql al servidor de hive con el siguiente comando:

``` sudo docker cp ./Paso02.hql hive-server:/opt/ ```

Ingresamos a Hive Server donde ademas podremos corroborar que el archivo se pasó correctamente:

```sudo docker exec -it hive-server bash ```

![image](https://github.com/EliLarregola/Herramientas_Big_Data/assets/91983204/1e7918c3-43df-46bb-a8e0-02f31cb09bb3)

Una vez dentro tendremos que ejecutar el archivo hql de la siguiente manera y con esto se crearán las tablas:

```hive -f Paso02.hql```

Para ingresar al entorno Hive se utiliza el comando:

```hive```

Ya dentro del entorno Hive, se puede trabajar con querys y realizar las consultas correspondientes con SQL. Recuerdo que a la hora de hacer un comando sin ";" el mismo contenedor te manda a la siguiente linea de comando

![image](https://github.com/EliLarregola/Herramientas_Big_Data/assets/91983204/aacd1f78-fe75-404c-b853-a860170ead11)
*Paso 2 Resultados de las consultas usando `show tables;` y `select * from sucursal;`* 


## PASO 3) FORMATOS DE ALMACENAMIENTO
El objetivo de este paso es almacenar en formato Parquet + Snappy las tablas creadas en el punto 2 a partir de archivos en formato csv. Tener en cuenta además de aplicar particiones para alguna de las tablas.

En el repositorio utilizado existe un archivo Paso03.hql. Este contiene una query que crea una base de datos llamada integrador 2, modifica la informacion y la guarda en formato Parquet con una compresion de tipo Snappy.

En primer lugar se pasa el archivo Paso03.hql al servidor de hive con el siguiente comando:

``` sudo docker cp ./Paso03.hql hive-server:/opt/ ```

Ingresamos a Hive Server donde ademas podremos corroborar que el archivo se pasó correctamente:

```sudo docker exec -it hive-server bash ```

Una vez dentro tendremos que ejecutar el archivo hql de la siguiente manera y con esto se crearán las tablas:

```hive -f Paso03.hql```






