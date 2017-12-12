# Instalación del Servidor de Indexado y Base de Datos

Las siguientes secciones indican el proceso para realizar la instalación y configuración de herramientas para el servidor de indexado y base de datos.

## Suposiciones
El sistema operativo considerado en las instrucciones de instalación es CentOs 7.0. Se asume que está instalado sin UI puesto que será administrado via terminal SSH.

Se asume también que el usuario que ejecuta la instalación cuenta con permisos de sudoer.

## Instalación de pre-requisitos

### Java Development Kit
El JDK de Java es erquerido para instalar el software de base. Se recomienda utilizar la distribución de Oracle.

Ejecute los siguientes comandos:

````
sudo yum update
cd ~
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/8u151-b12/e758a0de34e24606bca991d704f6dcbf/jdk-8u151-linux-x64.rpm"
````

Esto descargará el paquete de instalación. Una vez concluída la descarga ejecute los siguientes comandos:

````
sudo yum localinstall jdk-8u151-linux-x64.rpm
rm jdk-8u151-linux-x64.rpm
```` 

Compruebe la instalación ejecutando el comando <code>java --version</code>.

### MongoDB

Para la instalación de MongoDB es necesario agregar los repositorios dedicados de MongoDB. Para ello deberá crear un archivo para dicho repositorio.

Cree un archivo para edición con el siguiente comando: 

````
sudo vi /etc/yum.repos.d/mongodb-org.repo
````

Agregue el siguiente texto al archivo y guardelo.

````
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
````

Verifique que el repositorio aparezca en la lista resultante del comando <code>yum repolist</code>.

Ahora podrá instalar y activar el servicio de MongoDB con los siguientes comandos:

````
sudo yum -y install mongodb-org
sudo systemctl start mongod
````

Para el despliegue del Repositorio en ambientes productivos, es recomendado que el S.O. cuente con una partición dedicada para los datos. Esta partición debe ser configurada en el archivo de configuración de MongoDB.


### ElasticSearch 6.0

````
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.0.1.rpm
sudo rpm -ivh elasticsearch-1.7.3.noarch.rpm
````

ElasticSearch se instalará y los archivos de configuración serán colocados en la ruta _/etc/elasticsearch_.
Debido a que ElasticSearch sólo será accesible a través del API de indexado y búsqueda, no es necesario modificar la configuración por defecto.

En un ambiente productivo, deberá cofigurarse el nombre del nodo y del cluster de ElasticSearch. Se recomienda también utilizar una partición exclusiva para los datos en los índices.

### Apache Tomcat 8.5

````
mkdir
wget http://www-eu.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz
tar -xvzf apache-tomcat-8.5.24.tar.gz
sudo mv apache-tomcat-8.5.24.tar.gz /opt/tomcat
````
Lo anterior colocará el servidor de aplicaciones tomcat en la ruta _/opt_.


## Instalación de API de indexado

Para instalar el API de indexado deberá clonar el código del repositorio:

````
cd ~
git clone https://github.com/RepositorioNacionalCultura/EndPoint.git
````

Posteriormente deberá construir el API con los siguientes comandos:

````
cd EndPoint
mvn clean && mvn package
````

El paquete WAR del API se encontrará en _target/Search.WAR_. Este paquete será desplegado en el servidor Tomcat. Para ello, copie el archivo WAR a la ruta _/opt/tomcat/webapps_, renombrando el archivo como ROOT.

````
sudo cp target/Search.war /opt/tomcat/webapps/ROOT.war
````

Finalmente inicie el servidor Tomcat para iniciar la aplicación:

````
cd /opt/tomcat/bin
sh start.sh &
````

El API de indexado y busqueda respunderá peticiones en el puerto 8080. 
