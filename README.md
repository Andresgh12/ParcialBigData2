# Parcial Big Data 2: MapReduce con Hadoop en Docker

Este proyecto implementa un conteo de palabras sobre el archivo `apocalipsis.txt` que es sobre el libro apocalipsis de Stephen King utilizando Hadoop MapReduce 
en un entorno configurado con Docker. A continuación, se detalla cada paso del proceso, desde la instalación y 
configuración de Docker hasta la ejecución del trabajo MapReduce.

## Requisitos Previos
Antes de comenzar, asegúrate de tener lo siguiente instalado en tu máquina:
- **Docker Desktop**: Descargado, instalado y ejecutándose.
- **Git**: Instalado para clonar repositorios y gestionar el control de versiones.
- **Archivo de entrada**: `apocalipsis.txt` (incluido en el repositorio).

## Archivos en el Repositorio
- `apocalipsis.txt`: Archivo de texto usado como entrada para el conteo de palabras.
- `Resultados_Mapreduce`: Archivo de salida generado por el trabajo MapReduce con el conteo de palabras.
- `docker-compose.yml`: Archivo de configuración para el clúster Hadoop en Docker.

## Paso a Paso del Proceso

### 1. Clonar el Repositorio de Docker Hadoop
Clonamos el repositorio que contiene la configuración predefinida de Hadoop para Docker:
```bash
git clone https://github.com/big-data-europe/docker-hadoop.git
cd docker-hadoop
```
### 2. Iniciar el Clúster de Hadoop con Docker Compose
```bash
docker-compose up -d
```
### 3. Copiar el Archivo de Entrada (Apocalipsis.txt) al Contenedor Namenode
```bash
docker cp apocalipsis.txt namenode:/tmp/apocalipsis.txt
```
### 4.Acceder al Shell del Contenedor Namenode
```bash
docker exec -it namenode bash
```
### 5.Crear un Directorio en HDFS y Subir el Archivo de Entrada a HDFS
```bash
hdfs dfs -mkdir -p /user/root/input
hdfs dfs -put /tmp/apocalipsis.txt /user/root/input/
```
### 6.Ejecutar el Trabajo MapReduce de Conteo de Palabras
```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /user/root/input /user/root/output
```
Nota: La ruta del jar puede variar segun la version del hadoop instalada por lo que es mejor verificar cual se tiene por medio de:
```bash
find / -name "hadoop-mapreduce-examples-*.jar"
```
### 7.Copiar el archivo de salida del MapReduce al sistema de archivos local del contenedor
```bash
hdfs dfs -get /user/root/output/part-r-00000 /tmp/part-r-00000
```
### 8.Copiar el Archivo de Salida a la Máquina Local
```bash
docker cp namenode:/tmp/part-r-00000 "Ruta de donde queremos que se guarde"
```


