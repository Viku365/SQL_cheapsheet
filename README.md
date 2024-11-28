# SQL para Hive en HDInsight: Cheat Sheet

## Conectar a Hive en HDInsight

1. **Acceder a Hive a través de Ambari**: Navega a la interfaz web de Ambari para monitorear el estado del cluster.
2. **Usar Hive desde la CLI**: Conéctate al nodo principal del cluster y usa el comando `hive`.
3. **HiveView**: Puedes acceder a HiveView desde el portal de Ambari para ejecutar consultas más fácilmente.

## Creación de Bases de Datos y Tablas

- **Crear una base de datos**:
  ```sql
  CREATE DATABASE nombre_bd;
  ```

- **Usar una base de datos**:
  ```sql
  USE nombre_bd;
  ```

- **Crear una tabla**:
  ```sql
  CREATE TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT,
    columna3 DOUBLE
  )
  ROW FORMAT DELIMITED
  FIELDS TERMINATED BY ','
  STORED AS TEXTFILE;
  ```

- **Crear una tabla particionada**:
  ```sql
  CREATE TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT
  )
  PARTITIONED BY (year INT, month INT)
  STORED AS PARQUET;
  ```

- **Crear una tabla externa**:
  ```sql
  CREATE EXTERNAL TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT
  )
  ROW FORMAT DELIMITED
  FIELDS TERMINATED BY '\t'
  LOCATION 'ruta/a/datos/externos';
  ```

## Cargar Datos en Tablas

- **Cargar datos desde un archivo local**:
  ```sql
  LOAD DATA LOCAL INPATH 'ruta/a/archivo.csv' INTO TABLE nombre_tabla;
  ```

- **Cargar datos en una tabla particionada**:
  ```sql
  LOAD DATA LOCAL INPATH 'ruta/a/archivo.csv' INTO TABLE nombre_tabla PARTITION (year=2023, month=11);
  ```

## Consultas Básicas

- **Seleccionar datos**:
  ```sql
  SELECT columna1, columna2 FROM nombre_tabla WHERE columna2 > 100;
  ```

- **Contar registros**:
  ```sql
  SELECT COUNT(*) FROM nombre_tabla;
  ```

- **Ordenar resultados**:
  ```sql
  SELECT columna1, columna2 FROM nombre_tabla ORDER BY columna2 DESC;
  ```

- **Limitar resultados**:
  ```sql
  SELECT columna1, columna2 FROM nombre_tabla LIMIT 10;
  ```

## Filtrado de Datos

### Filtrado en columnas numéricas

- **Obtener los registros donde `columna2` sea mayor a 3**:
  ```sql
  SELECT * FROM nombre_tabla WHERE columna2 > 3;
  ```

- **Obtener los registros donde `columna2` esté entre 3 y 6**:
  ```sql
  SELECT * FROM nombre_tabla WHERE columna2 BETWEEN 3 AND 6;
  ```

### Filtrado en columnas de texto

- **Obtener los registros donde `columna1` contenga 'valor'**:
  ```sql
  SELECT * FROM nombre_tabla WHERE columna1 LIKE '%valor%';
  ```

- **Obtener los registros donde `columna1` empiece con 'pre'**:
  ```sql
  SELECT * FROM nombre_tabla WHERE columna1 LIKE 'pre%';
  ```

## Funciones de Agregación

- **SUM, AVG, MIN, MAX**:
  ```sql
  SELECT SUM(columna2), AVG(columna2), MIN(columna2), MAX(columna2)
  FROM nombre_tabla;
  ```

- **Agrupar por**:
  ```sql
  SELECT columna1, COUNT(*)
  FROM nombre_tabla
  GROUP BY columna1;
  ```

## Filtrado de Datos Agregados

- **Filtrar después de agrupar**:
  ```sql
  SELECT columna1, COUNT(*)
  FROM nombre_tabla
  GROUP BY columna1
  HAVING COUNT(*) > 10;
  ```

## Operaciones de Joins

- **Inner Join**:
  ```sql
  SELECT a.columna1, b.columna2
  FROM tabla1 a
  JOIN tabla2 b
  ON a.id = b.id;
  ```

- **Left Join**:
  ```sql
  SELECT a.columna1, b.columna2
  FROM tabla1 a
  LEFT JOIN tabla2 b
  ON a.id = b.id;
  ```

## Manejo de Tablas y Particiones

- **Agregar una partición a una tabla**:
  ```sql
  ALTER TABLE nombre_tabla ADD PARTITION (year=2023, month=11);
  ```

- **Mostrar particiones**:
  ```sql
  SHOW PARTITIONS nombre_tabla;
  ```

- **Mostrar las tablas disponibles**:
  ```sql
  SHOW TABLES;
  ```

- **Describir una tabla**:
  ```sql
  DESCRIBE nombre_tabla;
  ```

## Optimizar el Rendimiento

- **Usar Bucketing**: Mejora el rendimiento en tablas grandes.
  ```sql
  CREATE TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT
  )
  CLUSTERED BY (columna1) INTO 4 BUCKETS;
  ```

- **Ejecutar `ANALYZE` para estadísticas**:
  ```sql
  ANALYZE TABLE nombre_tabla COMPUTE STATISTICS;
  ```

- **Configurar `set` para optimización** (ejemplo: aumentar `mapreduce`):
  ```sql
  SET mapreduce.map.memory.mb=4096;
  SET mapreduce.reduce.memory.mb=4096;
  ```

## Manejo de Datos con ORC y Parquet

- **Crear tablas usando formatos ORC/Parquet** para mayor eficiencia:
  ```sql
  CREATE TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT
  )
  STORED AS ORC;
  ```

  ```sql
  CREATE TABLE nombre_tabla (
    columna1 STRING,
    columna2 INT
  )
  STORED AS PARQUET;
  ```

Estos son los comandos y conceptos más útiles para trabajar con Hive SQL en un entorno HDInsight. Ajusta según las necesidades de tu proyecto para aprovechar al máximo las capacidades del clúster. ¡Espero que te sea útil!

