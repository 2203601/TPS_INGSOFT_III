
Luciana Hueda (2203601) Sofia Tula --> repositorio --> https://github.com/2203601/2025_TP02_Docker_INGSOFTIII 


# Trabajo Práctico 02 – Introducción a Docker

### **1\. Elegir y preparar tu aplicación**

* Elegí una aplicación web para containerizar (puede ser propia, de un tutorial, o un proyecto simple)  
    
  Vamos a utilizar la aplicación web que realizamos en Arquitectura de Software II sobre una página de cursos online, donde el usuario puede ser administrador de la página o un estudiante. 


* Creá un repositorio en GitHub para tu proyecto  
  Creamos un repositorio llamado 2025\_TP02\_Docker\_INGSOFTIII desde la página web de github. Luego clonamos el repositorio con git clone y, ya finalizado, pegamos en la carpeta de aplicación todos los archivos necesarios para correr la misma.  
  Luego para que se suban los cambios al GitHub, hicimos un git add, git commit \-m “aplicación” y git push.  
    
* Configura tu entorno Docker   
  Descargamos Docker Desktop e instalamos, realizamos también el comando:  
  docker version.  
  Para asegurarnos que estaba instalado correctamente. 


### **2\. Construir una imagen personalizada**

* Creá un Dockerfile para la aplicación.  
* Elegí la imagen base que consideres más adecuada (justificá tu elección en decisiones.md).  
* Etiqueta la imagen con tu usuario de Docker Hub y un tag significativo.  
* Explicá en decisiones.md las instrucciones utilizadas y el porqué de la estructura del Dockerfile.  
  Nuestra aplicación está dividida en cursos-api, users-api, admin-api y search-api. Para este ejercicio, vamos a utilizar la api de usuarios.  
  El dockerfile está formado por:  
- **FROM**: define la imagen base sobre la que se va a construir el contenedor.   
- **WORKDIR**: cambia el directorio de trabajo dentro del contenedor. De acá, se ejecutan los comandos que siguen.  
- **COPY**: copia archivos o carpetas desde la máquina host al contenedor.   
- **RUN**: ejecuta un comando dentro de la imagen en tiempo de construcción.   
- **EXPOSE**: documenta el puerto que el contenedor usará para escuchar conexiones.  
- **CMD**: define el comando por defecto que se ejecuta cuando corre el contenedor.


  Dicho esto, el dockerfile para users-api se ve así:

  FROM golang:1.23-alpine


  WORKDIR /app


  COPY go.mod go.sum ./

  RUN go mod tidy


  COPY . .

  RUN go build \-o app ./main.go


  EXPOSE 8081


  CMD \["./app"\]

  Algunas de las explicaciones:

-  Utilizamos la imagen base **golang:1.23-alpine** ya que esta imagen base ya que incluye una versión moderna, con mayor compatibilidad y seguridad, de Go. Al estar basada en Alpine, distribución de Linux, hace que la imagen sea liviana y tenga menos superficie de ataque. Otra ventaja es que permite que la imagen venga con Go instalado y configurado, sin necesidad de hacerlo manualmente.  
- Establecemos el directorio de trabajo dentro del contenedor, /app, y se van a correr todos los comandos dentro de este mismo.  
- Con **COPY go.mod go.sum ./**, copiamos los archivos go.mod y go.sum al directorio de trabajo /app.  
- Con **RUN go mod tidy,** descargamos las dependencias que definimos en go.mod. Prepara el entorno de Go dentro del contenedor.  
- Ahora con **COPY ..** copiamos el resto de los archivos del proyecto en /app.  
- **RUN go build \-o app ./main.go**  permite compilar la aplicación Go.   
- Con **EXPOSE 8081** decimos el puerto en el que la aplicación escuchará dentro del contenedor.


  Para etiquetar una imagen con tu usuario de Docker Hub y un tag significativo, vamos a de nuevo utilizar la imagen de users-api.	

  Primero que todo, debemos hacer el login, para eso tenemos que correr **docker login.**

  Como nosotras ya tenemos creada la imagen, para agregar el tag tenemos que correr el comando: **docker tag users-api:latest usuario/users-api:v1** donde en usuario colocamos el usuario de Docker de cada una. Utilizamos v1 para indicar que es la primera versión estable. 

### **3\. Publicar la imagen en Docker Hub**

* Subí la imagen a tu cuenta de Docker Hub.  
* Explicá en decisiones.md la estrategia de versionado de imágenes.  
    
  Para publicar una imagen en Docker Hub, tenemos correr **docker push usuario/users-api:v1**  
  Para el versionado de imágenes, seguimos la semántica de (MAJOR.MINOR.PATCH) donde:  
- Major: cambios incompatibles o de arquitectura.   
- Minor: nuevas funcionalidades que mantienen compatibilidad.  
- Patch: correcciones menores o parches de seguridad.  
  Por eso, con v1.0.0 hacemos referencia a una primera versión estable.


### **4\. Integrar una base de datos en contenedor**

* Elegí la base de datos que prefieras (PostgreSQL, MySQL, MongoDB, SQL Server, etc.)  
* Montá un volumen persistente para datos.  
* Mostrar cómo conectaste la aplicación al contenedor de base de datos.  
* Justifica en decisiones.md por qué elegiste esa base de datos.  
    
  La api de usuarios ya tiene una base de datos MySQL, donde se guarda la información como el email, usuario id, etc.  
    
  Se definió un volumen persistente en el archivo **docker-compose.yml.**  
  Este volumen (mysql-data) está asociado al directorio donde MySQL almacena sus datos internos (/var/lib/mysql). De esta manera, los datos quedan guardados en el host y sobreviven a reinicios o recreaciones del contenedor.  
  La aplicación users-api se conecta al contenedor de MySQL a través de variables de entorno configuradas en el docker-compose.yml.  
    
  Entre ellas se definen el host, el puerto, el usuario, la contraseña y el nombre de la base de datos.

Se eligió MySQL como motor de base de datos por las siguientes razones:

Elegimos MySQL como motor de base de datos ya que la API de usuarios requiere almacenar información relacional, lo cual se adapta naturalmente al modelo relacional que ofrece MySQL. Tambien, Go dispone de un driver oficial usado para MySQL, además tiene una imagen oficial de MySQL en Docker Hub.

Para realizar la conexión de la aplicación al contenedor de base de datos, se realiza mediante Docker compose.yml.

mysql:                         

  image: mysql:latest   

  container\_name: mysql-container

  ports:

    \-  "3307:3306"              	

 environment:

    MYSQL\_ROOT\_PASSWORD: root1234   \# Clave del usuario root

    MYSQL\_DATABASE: users-api       \# Crea una DB inicial con ese nombre (te sugiero evitar guiones)

    MYSQL\_PASSWORD: root1234        \# Solo aplica si además defines MYSQL\_USER (ver corrección abajo)

  volumes:

    \- mysql-data:/var/lib/mysql     \# Volumen persistente: datos no se pierden al recrear el contenedor

  networks:

    \- app-network                 

  healthcheck:

    test: \["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root"\]

    timeout: 20s

    retries: 10

En el archivo docker-compose.yml se definió un servicio llamado **mysql**, que representa el contenedor donde corre la base de datos.

* **Imagen**: se utilizó la imagen oficial de MySQL disponible en Docker Hub (mysql:latest). Esta elección asegura compatibilidad y actualizaciones automáticas.

* **Nombre del contenedor**: se lo llamó mysql-container para identificarlo fácilmente al listar contenedores en ejecución.

* **Puertos**: se configuró el mapeo 3307:3306. Esto significa que dentro del contenedor MySQL escucha en el puerto **3306** (su puerto estándar), y desde la máquina anfitriona se accede a través del puerto **3307**. Así, si se quiere entrar desde el host, se hace en localhost:3307, mientras que las demás aplicaciones dentro de Docker se conectan directamente al puerto 3306 interno.

* **Variables de entorno**: se declararon parámetros necesarios para inicializar MySQL: la contraseña del usuario root, el nombre de la base de datos inicial y la contraseña para acceder. Estas variables son interpretadas automáticamente por la imagen oficial y permiten que el contenedor arranque ya con una base creada.

* **Volumen**: se definió un volumen persistente (mysql-data) asociado a la carpeta /var/lib/mysql del contenedor. Esto garantiza que los datos de la base se conserven aunque el contenedor sea eliminado o reiniciado.

* **Red**: el servicio se incluyó dentro de la red interna app-network. Esto permite que otros contenedores (como users-api) puedan comunicarse con la base de datos usando directamente el nombre del servicio (mysql) como host, gracias al DNS interno de Docker.

* **Healthcheck**: se añadió un chequeo de salud que ejecuta un comando de ping a MySQL. Esto sirve para que Docker verifique cuándo el contenedor de la base está listo para recibir conexiones, evitando que las aplicaciones dependientes intenten conectarse antes de tiempo.

Este bloque asegura que el contenedor de MySQL se inicie correctamente, que sus datos persistan mediante un volumen, y que las demás aplicaciones de la solución (por ejemplo users-api) puedan conectarse fácilmente indicando en sus variables de entorno que el host de la base es **mysql** dentro de la red de Docker.

### **5\. Configurar QA y PROD con la misma imagen**

* Usá variables de entorno para que la misma imagen corra con diferentes configuraciones según el entorno (ej: cadenas de conexión, modos de log, etc.).  
* Correr dos contenedores simultáneamente (uno QA y uno PROD) usando la misma imagen, cada uno con su configuración.  
* Justificar en decisiones.md cómo definiste las variables de entorno y cómo se aplican.

Las ***variables de entorno*** en Docker son pares clave-valor que permiten personalizar la configuración de una aplicación dentro de un contenedor, facilitando el ajuste de su comportamiento en diferentes entornos sin modificar la imagen de Docker. 

Para configurar dos contenedores que corran con la misma imagen a partir de las variables de entorno, tenemos que modificar el docker-compose.yml que habíamos creado previamente para el proyecto.

En dicho archivo vamos a agregar las siguientes cosas:

- users–api QA  
   users-api-qa:  
     image: users-api:latest  
     container\_name: users-api-qa  
     build:  
       context: ./users-api  
       dockerfile: Dockerfile

  command: /bin/sh \-c "sleep 20 && until nc \-z mysql 3306; do sleep 1; done && go run main.go"

     ports:  
       \- "8087:8081"  
     depends\_on:  
       \- mysql  
       \- memcached  
     environment:  
       \- GO111MODULE=on  
       \- ENV=QA  
       \- APP\_ENV=qa  
       \- DB\_HOST=mysql  
       \- DB\_NAME=users-api  
       \- DB\_USER=root  
       \- DB\_PASSWORD=root1234  
       \- LOG\_LEVEL=debug  
     networks:  
       \- app-network  
     restart: on-failure

	Explicaciones:

- El **nombre del servicio** es *users-api-qa* el cual tiene como **imagen** *users-api:latest* donde latest es la versión de la imagen.  El **nombre del contenedor** será *users-api-qa*.  
- Pata poder construir la imagen. le decimos que debe tener como **contexto** *./users-api* (donde se encuentra el código fuente) y utilizar el archivo **Dockerfile** que se encuentra en dicha carpeta.  
-  La linea *command: /bin/sh \-c "sleep 20 && until nc \-z mysql 3306; do sleep 1; done && go run [main.go](http://main.go)"* básicamente dice que debe esperar 20 segundos antes de arrancar, hacer ping al servicio mysql en el puerto 3306 hasta que esté disponible y recien ahi ejecutar el código.  
- Utiliza el **puerto** *8087:8081*  donde 8087 es el puerto interno del contenedor y 8081 el puerto expuesto a la máquina.   
- Luego tenemos las **dependencias**, donde dejamos entender que este servicio no se puede levantar si no se levantaron previamente *mysql y memcached.*  
- El apartado **environment** hace referencia a las variables de entorno, en este tenemos:  
  - *GO111MODULE=on* que habilita Go Modules, el manejo de dependencias de Go.  
  - *ENV=QA / APP\_ENV=qa* que definen el entorno de ejecución, en este caso, QA.  
  - Luego tenemos todo relacionado a la base de datos como su contraseña, el host, el nombre de la misma.  
  - *LOG\_LEVEL=debug* que permite más detalle en los logs.  
- En **network** permitimos que el servicio users-api-qa se conecte a una red personalizada y se poder comunicarse con mysql y memcached usando sus nombres de servicio como hostnames.  
- Lo que sería *restart: on failure*  dice que si el contenedor se cae, Docker lo vuelve a levantar.  
  Para users-api-prod, debemos tener lo mismo pero cambiando lo que serían las variables de entorno.   
   users-api-prod:  
     image: users-api:latest  
     container\_name: users-api-prod  
     build:  
       context: ./users-api  
       dockerfile: Dockerfile

  command: /bin/sh \-c "sleep 20 && until nc \-z mysql 3306; do sleep 1; done && go run main.go"

     ports:  
       \- "8088:8081"  
     depends\_on:  
       \- mysql  
       \- memcached  
     environment:  
       \- GO111MODULE=on  
       \- **ENV=PROD**  
       **\- APP\_ENV=prod**  
       \- DB\_HOST=mysql  
       \- DB\_NAME=users-api  
       \- DB\_USER=root  
       \- DB\_PASSWORD=root1234  
       \- LOG\_LEVEL=error  
     networks:  
       \- app-network  
     restart: on-failure  
    
  La diferencia entre **ENV**  y **APP\_ENV** es que:  
- **ENV:** es genérica, conocida por muchas librerías.  
- **APP\_ENV:** es específica para el proyecto, manejo la lógica interna.

### **6\. Preparar un entorno reproducible con docker-compose**

* Creá un archivo docker-compose.yml que levante:  
  * La app en QA  
  * La app en PROD  
  * La base de datos correspondiente  
* Configurá volúmenes y variables de entorno en docker-compose.yml.  
* Documentá cómo aseguramos que este entorno se ejecuta igual en cualquier máquina.


Como explicamos previamente, en el **docker compose** ponemos los servicios necesarios para poder levantar la aplicación y que funcione correctamente. De este modo, vamos a tener los servicios que explicamos anteriormente como mysql, users-api-qa, users-api-prod y los otros servicios que creamos en su momento para que funcione la página.

Primero debemos poner el tipo de **versión** del docker compose. En nuestro archivo, utilizamos la versión *3.8.* Este define la sintaxis y features que podemos usar. 

Luego tenemos el apartado de **servicios** donde se encuentra todo lo que serían los servicios explicados previamente como mysql, users-api-qa y users-api-prod. También se encuentran los servicios: memcached, mongo, rabbitmq, solr, nginx, cursos-api, search-api, frontend1, admin-api.

Después tenemos los apartados de network y volumes:

- **network:** acá declaramos las redes personalizadas que utilizamos para conectar los contenedores entre sí. Los servicios como users-api y mysql utilizan la red app-network. Se usa el driver por defecto de Docker, que crea una red virtual privada dentro del host.  
- **volumes:** se declaran los volúmenes que van a ser gestionados por Docker.  Es un espacio de almacenamiento que vive fuera del ciclo de vida del contenedor. Aunque se borre el contenedor, los datos siguen ahí. Por eso, creamos un volumen para guardar los datos de mysql.

Para garantizar que un entorno de pruebas (QA) se ejecute de forma idéntica en cualquier máquina, independientemente de, librerías locales o diferencias de configuración, utilizamos Docker y Docker Compose, que nos permiten definir y levantar todo el entorno QA de manera declarativa, portable y determinista.

Docker empaqueta la aplicación, dependencias, librerías y la configuración necesaria en contenedores. Esto asegura que el entorno QA se ejecute igual en windows, linux, o macOS.

Todo el stack (aplicación, base de datos, caché, etc) corre dentro de contenedores aislados, eliminando dependencias externas del sistema host.

*Orquestación con docker compose*  
El archivo [docker-compose.qa](http://docker-compose.qa).yml describe todos los servicios que conforman en entorno QA:

- users-api   
- mysql  
- memcached

Con el comando: docker compose \-f docker-compose.qa.yml up \-d  
Se levantan todos los servicios con la misma configuración en cualquier máquina que tenga Docker instalado.

La aplicación se construye mediante un `Dockerfile`, donde se especifica la versión de Go y las dependencias necesarias.

Utilizamos el comando docker compose up \-d dado que es el encargado de poner en marcha todo el entorno definido en el archivo docker-compose.yml  
up \= crea los contenedores, redes, y volúmenes necesarios y arranca cada servicio.  
\-d \= Indica que los contenedores se ejecutan en segundo plano, sin bloquear la terminal.

Entonces este comando deja los contenedores en ejecución, redes internas controladas, volúmenes de datos persistentes, puertos expuestos en el host.  
Se genera entonces en la máquina un entorno completo de QA y PROD listo para usar, con la seguridad de que será el mismo para cualquier integrante del equipo que lo ejecute

### 

### **7\. Crear una versión etiquetada**

* Etiquetá la imagen de la aplicación con un tag v1.0 y actualizá el docker-compose.yml para usar esa versión.  
* Explica la convención de versión elegida.

Para etiquetar una imagen de la aplicación con un tag debemos correr el siguiente comando:  ***docker build \-t users-api:v1.0 ./users-api .*** Luego debemos ir al apartado de users-api-qa y users-api-prod y cambiar la **imagen** a *users-api:v1.0.*

Como antes, para el versionado de imágenes, seguimos la semántica de (MAJOR.MINOR.PATCH) donde:

- Major: cambios incompatibles o de arquitectura.   
- Minor: nuevas funcionalidades que mantienen compatibilidad.  
- Patch: correcciones menores o parches de seguridad.


Por eso, con v1.0.0 hacemos referencia a una primera versión estable.

