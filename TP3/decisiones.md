Luciana Hueda (2203601) --> repositorio --> https://github.com/2203601/ING-SOFT-III/tree/main/tp3
						--> proyecto --> https://dev.azure.com/lucianahueda13/Trabajo-Practico-III
						
Sofia Tula (2320454) --> repositorio --> https://github.com/SofiaTula/IngeSoft3/tree/main/tp3 
					 --> proyecto --> https://dev.azure.com/sofitula7/Trabajo-Practico-III 


***TRABAJO PRÁCTICO 3*** 

### **1\. Configuración inicial del proyecto**

* Crear una organización en Azure DevOps  
  Para crear una organización en DevOps debemos seleccionar la opción de crear nueva organización. Ahí vamos a elegir:  
- Nombre de la organización: [dev.azure.com/](http://dev.azure.com/) \-nombre-  
- El país en donde van hacer el host del proyecto, nosotras elegimos Estados Unidos.  
- Y después te pide que escribas un código para confirmar que no seas un robot.  
    
* Crear un proyecto con la metodología ágil que consideres apropiada.  
  Luego vamos a seleccionar crear un nuevo proyecto. En dicho apartado, debemos completar con el nombre del proyecto y, si queremos, una descripción.  
    
  Elegimos la metodología ágil porque tiene mayor flexibilidad y adaptación al cambio y en proyectos de desarrollo como el nuestro, las prioridades pueden cambiar, por lo tanto, la metodología ágil permite reorganizar y ajustar sin necesidad de rehacer todo.   
  También es mejor a la hora de colaboración constante dado que fomenta la comunicación continua entre los equipos y responsables de negocio, cuenta además con simplicidad en la gestión y comparando con otras metodologías más pesadas, ágil permite mantener el foco en avanzar y entregar resultados, sin exceso de documentación, es ideal cuando el equipo necesita velocidad y flexibilidad.  
  Ágil también puede escalar fácilmente aunque empecemos con equipos pequeños. Cada uno mantiene su propio tablero pero todos trabajan alineados bajo el mismo marco.  
  En conclusión, elegimos Ágil porque nos permite trabajar de manera iterativa, flexible y colaborativa.  
   
    
    
* Configurar los equipos y áreas del proyecto

	Creamos los siguientes equipos:

- Frontend.  
- Backend.  
- Base de datos.  
  Para crearlos, debemos ir a configuración del proyecto, equipos y crear nuevo equipo. Las áreas se van a crearon al mismo tiempo que se crearon los equipos.	

    -- VER EN /img img-01 --
	

### **2\. Gestión del trabajo con Azure Boards**

Para poder crear todos estos work items, debemos ir a boards, work items y luego new work item.

* Crear al menos:  
  * 1 Epic que represente una funcionalidad completa

    Para el Epic, creamos uno que se llama Gestión de Usuarios  donde es la funcionalidad completa que incluye registro, login y administración de usuarios.

    

  * 3 User Stories relacionadas con el Epic

			Creamos 3 historias de usuario:

- ***US-01 REGISTRO:***  
  - **Descripción**: Como visitante quiero registrarse con email y contraseña para poder acceder a la aplicación.  
  - **Criterios de aceptación:**   
    - El usuario debe ingresar un email válido.   
    - La contraseña debe tener mínimo 8 caracteres.   
    - El sistema debe mostrar un mensaje de confirmación al registrarse.  
  - **Área**: General (Trabajo-Práctico-III)  
  - **Prioridad:** 1

    

- ***US-02 LOGIN:***  
  - **Descripción**: Como usuario registrado quiero iniciar sesión con mis credenciales para poder acceder a mi cuenta personal.  
  - **Criterios de aceptación:**   
    - El sistema debe validar email y contraseña contra los registros existentes en la base de datos.  
    - Si las credenciales son correctas, el usuario accede a la aplicación.   
    - Si las credenciales son incorrectas, se muestra un mensaje de error.   
    - El sistema debe mantener la sesión activa hasta que el usuario cierre sesión.   
  - **Área**: General (Trabajo-Práctico-III)  
  - **Prioridad:** 2

    

- ***US-03 ADMINISTRACIÓN DE USUARIOS (admin)***  
  - **Descripción**:Como administrador quiero poder ver, editar y deshabilitar usuarios para gestionar la plataforma.   
  - **Criterios de aceptación:**   
    - El administrador debe poder visualizar una lista de usuarios registrados.  
    - El administrador debe poder editar la información de un usuario (ej: nombre, email, rol).  
    - El administrador debe poder deshabilitar cuentas de usuario.  
    - El sistema debe registrar la fecha y hora de la última modificación hecha por el administrador.  
  - **Área**: General (Trabajo-Práctico-III)  
  - **Prioridad:** 2

    

		Para relacionarlas con el Epic, debemos hacer lo siguiente:

1. Ir a boards, work items y seleccionar el epic, en nuestro caso, es  Gestión de Usuarios.  
2. Seleccionamos Links → Add link → Existing Item:  
   1. En tipo de link: affected by ya que el Epic se va a ver afectado si se cumple las distintas historias de usuarios.  
   2. En work items to link, seleccionamos todas las historias de usuario que queremos.

		Nos quedaría así:
	    -- VER EN /img img-02 --
		

![][image2]

* 2 Tasks por cada User Story  
  Para crear Tasks, debemos ir a Boards → Work Items → New Work Items → Task.  
  Creamos los siguientes tasks:  
- *Para US-01 REGISTRO:*  
  * ***Task 1: Crear Formulario de registro.***  
    * **Descripción**: Diseñar e implementar la interfaz de usuario para el registro de nuevos usuarios, incluyendo campos como nombre, email, contraseña y confirmación de contraseña. Debe ser responsivo, accesible y amigable para el usuario.  
    * **Área**: Frontend.  
  * ***Task 2: Implementar API de registro y validación***  
    * **Descripción**: Crear la lógica del lado del servidor para recibir los datos del formulario, validar la información (longitud de contraseña, formato de email, duplicados), guardar el usuario en la base de datos y retornar mensajes de éxito o error adecuados.  
    * **Área**: Backend.

      

- *US-02 INICIO DE SESIÓN:*  
  * ***Task 1: Pantalla de Login***  
    * **Descripción**: Diseñar e implementar la pantalla de inicio de sesión con campos para email y contraseña, botones de “Ingresar” y “Recuperar contraseña”, y mensajes de error claros si los datos son incorrectos.  
    * **Área**: Frontend.  
  * ***Task 2: Autenticación de credenciales***  
    * **Descripción**: Implementar la lógica de autenticación que verifique el email y contraseña ingresados, genere tokens de sesión seguros, y maneje errores de credenciales incorrectas.  
    * **Área**: Backend.

      

- *US-03 ADMINISTRACIÓN DE USUARIOS:*  
  * ***Task 1: Crear vista de administración.***  
    * **Descripción**: Desarrollar una interfaz de administración para que el administrador pueda ver la lista de usuarios, buscar usuarios específicos, y acceder a funciones como editar, deshabilitar o eliminar cuentas.  
    * **Área**: Frontend.  
  * ***Task 2: Implementar endpoints CRUD***  
    * **Descripción**: Crear endpoints de tipo CRUD (Crear, Leer, Actualizar, Eliminar) que permitan al administrador gestionar los usuarios. Esto incluye la deshabilitación de cuentas, actualización de datos y eliminación segura de usuarios.  
    * **Área**: Backend.

    Para relacionar cada task con su historia de usuarios correspondiente, debemos hacer lo siguiente:

* Ir a boards, work items y seleccionar la historia de usuario.

  Seleccionamos Links → Add link → Existing Item:

  * En tipo de link: affected by ya que la historia de usuario se va a ver afectada si se cumple los distintos tasks.  
  * En work items to link, seleccionamos todos los tasks que le corresponden.

    

  * 2 Bugs de ejemplo  
    Para crear un Bug, debemos ir a Boards → Work Items → New Work Items → Bug.  
    Creamos los siguientes Bugs:  
- B1 \- Contraseña demasiado corta:  
  * Info del Sistema: El sistema permite registrar usuarios con contraseñas menores a 8 caracteres, incumpliendo el criterio de aceptación.  
  * Área: Backend.  
  * Afecta al task de Implementar API de registro y validación.  
- B2 \-  Botones de acción no funcionan:  
  * Info del Sistema: Los botones para editar o deshabilitar usuarios no responden al clic o generen errores en la pantalla.  
  * Área: Frontend.  
  * Afecta al task de crear vista de administración.  
    Para relacionar cada bug con su task correspondiente, debemos hacer lo siguiente:  
* Ir a boards, work items y seleccionar el task.  
* Seleccionamos Links → Add link → Existing Item:  
  * En tipo de link: affected by  
  * En work items to link, seleccionamos todos los bugs que le corresponden.  
    Por ejemplo, el task de Crear vista de administración, nos quedaría así:  
   -- VER EN /img img-03 --
	
* Configurar un Sprint de 2 semanas  
  Para crear un Sprint, tenemos que ir a Boards → Sprint → New Sprint.  
  En dicho apartado, ponemos el inicio y fin del sprint y para qué área sería.  
  Nuestro nuevo sprint se llama: Sprint 1 \- Gestión de Usuario y va desde el 3 de septiembre hasta el 17 de septiembre.

   -- VER EN /img img-04 --

* Asignar los work items al Sprint  
  Para asignar el Sprint a cada work items, tenemos que ir a Boards → Seleccionar el work item que queramos → Iteration y ahí seleccionamos el sprint.  
   -- VER EN /img img-05 --
  Y hacemos este proceso con cada work item.  
    
* Documentar la estructura de trabajo elegida  
  La estructura elegida es la siguiente:  
* **Epic** → Representa un bloque grande de funcionalidad.  
  * **User Story (US)** → Funcionalidad específica que aporta valor al usuario.  
    * **Task (T)** → Trabajo técnico necesario para implementar la US.  
    * **Bug (B)** → Error o falla encontrada, relacionado a una Task o US.

  Ejemplo visual:

  **Epic: Gestión de Usuarios**

       **├── US-01: Registro de usuarios**

       **│     ├── Task 1: Diseñar formulario de registro (Frontend)**

       **│     ├── Task 2: Implementar validación y guardado en DB (Backend)**

       **│     ├── Bug 1: Contraseña menor a 8 caracteres permitida (Backend)**

       **│     └── Bug 2: Email duplicado permitido (DB)**

       **│**

       **├── US-02: Inicio de sesión**

       **│     ├── Task 1: Implementar formulario de login (Frontend)**

       **│     ├── Task 2: Validar credenciales y gestionar sesión (Backend)**

       **│     ├── Bug 1: Login con email inexistente (DB)**

       **│     └── Bug 2: Sesión se cierra antes de tiempo (Backend)**

       **│**

       **└── US-03: Administración de usuarios**

             **├── Task 1: Crear vista de administración (Frontend)**

             **├── Task 2: Implementar endpoints CRUD (Backend)**

             **├── Bug 1: Administrador no puede deshabilitar usuarios (Backend)**

             **├── Bug 2: Cambios editados no se guardan (DB)**

             **├── Bug 3: Eliminación de usuario falla silenciosamente (Backend)**

             **└── Bug 4: Lista de usuarios no carga (Frontend)**

### **3\. Control de versiones con Azure Repos**

* Importar o crear un repositorio con código de una aplicación  
  Para realizar este paso lo que hicimos fue:

  \-Ir al panel izquierdo y seleccionar Repos, luego files y nos deriva a la siguiente página:  
   -- VER EN /img img-06 --

    
- Luego creamos una carpeta llamada Tp3 donde hicimos un git clone del repositorio.

* Configurar políticas de branch para la rama principal:  
  Primero debemos ver si existe una branch main en Repos → Files. Si el repo está vacío, debemos ir al final y hacer click en Initialize. Así, se creará un [README.md](http://README.md) desde la página.   
  Si entramos a branch podemos observar que se realizo con exito:  
   -- VER EN /img img-07 
   
  Ahora que tenemos una branch main, vamos a crear las ramas de feature, para eso corremos los siguientes comandos:

  git checkout \-b feature/registro-usuarios  
  git push origin feature/registro-usuarios  
  git checkout \-b feature/login  
  git push origin feature/login

   -- VER EN /img img-08 --


	Ahora volvemos Repos → Branches, y hacemos click en Branch Policies, y ponemos:

- Require a minimum number of reviewers. Lo desactivamos ya que como solo es una persona en el proyecto, no necesitamos que otro usuario lo apruebe.  
- Check for linked work items: Required. Esto es para chequear los work items que estén relacionados con los pull requests.   
- Y en Limit Merge types permitimos todos los tipos de merge.

* Realizar cambios y crear Pull Requests  
  En el documento de README, agregamos un cambio en feature/login.  
  Ahora vamos  a **Repos → Pull Requests → New Pull Request**.  
  Configurar:  
- **Source branch:** tu rama de feature (`feature/nombre-feature`)  
- **Target branch:** `main`  
- Agregá un **título** y una **descripción** del PR.  
- Asociar **Work Items** si corresponde.  
  Luego solo aprobamos el pull request y creamos el merge.


   -- VER EN /img img-09 - img-10 --


