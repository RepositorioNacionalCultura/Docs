# Instalación del Servidor de Front-End

Las siguientes secciones indican el proceso para realizar la instalación y configuración de herramientas para el servidor de indexado y base de datos.

## Suposiciones
El sistema operativo considerado en las instrucciones de instalación es CentOs 7.0. Se asume que está instalado sin UI puesto que será administrado via terminal SSH.

Se asume también que el usuario que ejecuta la instalación cuenta con permisos de sudoer.

Se asume conocimiento en administración del CMS SemanticWebBuilder.

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

### Apache Tomcat 9.0

````
mkdir
wget http://www-us.apache.org/dist/tomcat/tomcat-9/v9.0.2/bin/apache-tomcat-9.0.2.tar.gz
tar -xvzf apache-tomcat-9.0.2.tar.gz
sudo mv apache-tomcat-9.0.2.tar.gz /opt/tomcat
````
Lo anterior colocará el servidor de aplicaciones tomcat en la ruta _/opt_.

## Instalación de SemanticWebBuilder

Para instalar SemanticWebBuilder, deberá descargar el paquete correspondiente.

````
cd ~
wget https://github.com/SemanticWebBuilder/SWB/releases/download/5.0/swb.war
````

Este paquete será desplegado en el servidor Tomcat. Para ello, copie el archivo WAR a la ruta _/opt/tomcat/webapps_, renombrando el archivo como ROOT.

````
sudo cp target/swb.war /opt/tomcat/webapps/ROOT.war
````

Finalmente inicie el servidor Tomcat para iniciar la aplicación:

````
cd /opt/tomcat/bin
sh start.sh &
````

SemanticWebBuilder responserá en el puerto 8080.

