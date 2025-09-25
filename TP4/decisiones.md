Luciana Hueda (2203601) --> repositorio --> https://github.com/2203601/ING-SOFT-III/tree/main/tp4
						--> proyecto --> https://dev.azure.com/lucianahueda13/Trabajo-Practico-IV
						
Sofia Tula (2320454) --> repositorio --> https://github.com/SofiaTula/IngeSoft3/tree/main/tp4
					--> proyecto --> https://dev.azure.com/sofitula7/Trabajo-Practico-IIII 




# ***Trabajo Práctico 04 – Azure DevOps Pipelines*** 

### ***1\. Preparación del entorno***

* Crear Pool y Agente Self‑Hosted en ADO (instalado como servicio en tu máquina).

### Vamos a ir a **Project Settings → Agent Pool → Add pool**, en dicho apartado, nos va a pedir lo siguiente:

- Pool to link: New  
- Pool Type: Self Hosted. Esta opción nos permite crear un pool de agentes customizados y hosteados con nuestra infraestructura para un mayor control y flexibilidad.  
- Name: SelfHosted  
- Marcamos Grant access permission to all pipelines para que pueda manejar todas los pipelines.

Una vez creado, debemos crear un agente como servicio. Para ello, vamos a ir a nuestra pool y hacer click en New Agent. Ahí debemos seguir los siguientes pasos:

1. Descargar el agente.  
2. En la terminal:  
   1. \~/$ mkdir myagent && cd myagent  
   2. tar zxvf \~/Downloads/vsts-agent-osx-x64-4.260.0.tar.gz  
   3. sudo xattr \-r \-d com.apple.quarantine \~/myagent  
   4. ./[config.sh](http://config.sh)  
      1. Aceptamos el acuerdo Team Explorer Everywhere license  
      2. Agregamos el URL de nuestra organización.  
      3. Luego nos pide un Personal Access Token. Para conseguirlo, debemos ir a ADO, user setting, personal access tokens, new.  
      4. Copiamos el PAT y lo pegamos.  
      5. Ingresamos el nombre del Pool  
      6. Ingresamos nombre del agente, en este caso es mac-agent-01  
      7. Dejamos como predeterminado la carpeta de trabajo \_work.  
      8. Y corremos lo siguiente para que quede como servicio y siempre esté online:  
         1. sudo ./[svc.sh](http://svc.sh) install  
         2. sudo ./[svc.sh](http://svc.sh) start
   -- VER EN /img img-01 y img-02--


### ***2\. Estructura del repo y definición del pipeline***

* Organizar /front, /back y agregar azure-pipelines.yml en raíz.  
* YAML requerido (multi‑stage)

		Stage CI (trigger en main):

* Build front (por ejemplo npm ci && npm run build).  
  * Build back (por ejemplo dotnet restore/build/test o mvn package/gradle build).  
    * Publicación de artefactos (dist/bin).

Como repositorio, reutilizamos el proyecto de arquitectura de software II donde tenemos un backend dedicado a la administración de cursos y la posibilidad de inscribirse a cursos como estudiante; y luego un frontend que nos permite visualizar todo esto. A este repositorio, agregamos un file denominado azure-pipelines.yml, tal que nos quedaría algo así:
   -- VER EN /img img-03 --



Ahora agregamos el repositorio a nuestro proyecto de Azure DevOps. Para eso hacemos: Repos → Import Repo → pegamos el link para clonar nuestro repo de GitHub.

Para crear un pipeline, debemos ir a Pipelines → New Pipeline.

- Elegimos GitHub YAML ya que ahí se encuentra nuestro código.  
- Luego elegimos nuestro repositorio.  
- Para la configuración de pipeline, elegimos Existing Azure Pipelines YAML file y seleccionamos nuestro file azure-pipelines.yml  
- Tocamos en Run.

Al principio, al ser la primera vez, no va a correr el pipeline ya que debemos solicitar autorización para poder correr el pipeline.

El archivo azure-pipelines.yml esta seccionado de la siguiente forma:  
   -- VER EN /img img-04 --

Donde el ***trigger: main*** que indica que el pipeline se va a ejecutar automáticamente cada vez que haya un commit en la rama main. Después, configuramos el ***pool*** y ponemos el nombre del pool que creamos anteriormente, es decir, ***SelfHosted***.

   -- VER EN /img img-05 --

Ahora se encuentra la sección de stages, donde vamos a encontrar el stage con su nombre y todos los jobs que debe realizar el pool.  Definimos la etapa del pipeline que se llama CI por Continuous Integration y le ponemos como nombre Build CI (que es lo que vemos en el pipeline).

Para los jobs, tenemos bastantes que se dividen para frontend y para las distintas partes del backend ya que este está formado por users, admin, cursos, etc.

Dicho eso, nos quedaría así:

- Frontend:

   -- VER EN /img img-06 --
Donde:

* Le damos como nombre de job BuildFrontend y el displayName, que vemos en el pipeline, Build Frontend.  
* Luego configuramos los distintos steps que debe realizar:  
  * Primero va a instalar [Node.js](http://Node.js) version 18 en el agente  
  * Luego va a ir a la carpeta de frontend, y va a instalar las dependencias y compilarlo.  
  * Va a mostrar los archivos generados para la depuración.  
  * Y luego publica la carpeta build como un artefacto de compilación, permitiendo que otras etapas o pipelines puedan descargar el build compilado.  
- Backend (para cada api del mismo):

   -- VER EN /img img-07 --

	Donde:

* Le damos como nombre de job BuildAdminApi y el displayName, que vemos en el pipeline, Compilar el admin-api.  
* Luego configuramos los distintos pasos que debe realizar:  
  * Primero debe instalar Go 1.24 usando Homebrew. || true evita que falle si Go ya está instalado. Se agrega al PATH para que se pueda utilizar en el build.  
  * El script lo que hace es cambiar el directorio del backend, descargar las dependencias y limpiar el módulo Go; crea un directorio para guardar el binario. Luego lo compila y genera el ejecutable.  
  * Pública el binario compilado como artefacto para las otras etapas.


  Para cursos api, search api y users api; es lo mismo que antes pero cambiando los nombres y directorios.

Luego, cuando hayamos finalizado el código, debemos realizar un git add . → git commit \-m “” → git push. De esta forma, se va a correr el pipeline y crear los artefactos. Nos debería quedar algo asì:
   -- VER EN /img img-08 --

