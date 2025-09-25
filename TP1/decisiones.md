Luciana Hueda (2203601) --> repositorio --> https://github.com/2203601/2025_TP01_RepoBase 
Sofia Tula (2320454) --> repositorio --> https://github.com/SofiaTula/2025_TP01_RepoBase 



1. Configurar tu entorno y preparar tu repositorio

Cloná o forkeá el repositorio base https://github.com/ingsoft3ucc/2025_TP01_RepoBase
	- Corri el comando git clone https://github.com/ingsoft3ucc/2025_TP01_RepoBase.git en la terminal en la ubicación de mi carpeta de la materia.
	
Configurá tu identidad y dejá constancia en el archivo decisiones.md de cómo lo hiciste.
	- Para configurar la identidad, realice:
		- git config --global user.name "2203601"
		- git config --global user.email "2203601@ucc.edu.ar"


2. Desarrollar una funcionalidad

Trabajá en una rama separada de main.
	- Creamos una rama: git branch LuHueda
	- Nos movemos a esa branch: git checkout LuHueda
	
Hacé al menos 2 commits atómicos con mensajes claros.
	- Primero modifico app.s y agregue una función de sumar. Para el commit de esta modifición, voy hacer:
		- cd src (me muevo a la carpeta donde se encuentra app.js)
		- git add app.js
		- git commit -m "Agregamos funcion para sumar"
		- git push (para que se pueda ver desde GitHub en mi branch)
	- Ahora agrego otra función que es de restar, seguimos los mismo pasos anteriores.
		- git add app.js
		- git commit -m "Agregamos funcion para restar"
		- git push (para que se pueda ver desde GitHub en mi branch)

Justificá la estrategia que usaste (¿por qué esa rama? ¿por qué esos commits?).
	-Elegí la rama LuHueda ya que puede haber errores con las funciones agregadas, entonces para no afectar el branch 	main (que es más importante), lo hacemos en una branch distinta.
	- Los commits que realice son bastante simples pero dicen exactamente que es lo que se cambio. No hay problemas para 	entender los cambios.



3. Corregir un error (simulado) y aplicar el fix

Simulá un error en main y resolvelo en una rama hotfix.
	- Primero creamos la rama hotfix: git branch hotfix.
	- Ahora vamos a movernos al main y generar un error random como por ejemplo un error de tipeo, en la linea de     	console.log("Hola mundo") ahora dice consol.log("Hola mundo"). Realice el commit y push para que se encuentre en el 	repositorio.
	- Ahora nos movemos a la rama de hotfix y corregimos el error de tipeo (consol a console). Luego hacemos:
		- git add app.js
		- git commit "Se corrigio el error de tipeo"
		- git push.

Aplicá el fix a main y también a tu rama de desarrollo.

	- Agregamos el error del main a mi rama de desarrollo LuHueda con: git merge main 
	- Ahora tenemos que aplicar el fix al main y a la rama de desarrollo, para eso hacemos:
		- git checkout main, git merge origin/hotfix, git push.
		- git checkout LuHueda, git merge origin/hotfix, git push.


4. Hace un PR y aceptalo

Para hacer el Pull Request voy a GitHub a mi repositorio y voy a mi branch LuHueda.
	- Luego voy a pull request y toco en New Pull request.
	- Tengo que poner como base el main y compare LuHueda, y creo el pull request.
	- Como me permitio realizar el request, me voy ahora a mi branch main.
	- Voy para pull request y me va aparecer el request que hice anteriormente.
	- Acepto el pull request.

5. Crear una versión etiquetada

Marcá una versión estable con el tag v1.0.
	- La versión estable va a ser mi main.
	- git tag -a v1.0 -m "Versión estable inicial"
	- Ahora subo el tag a GitHub: git push origin --tags
	- vx.y: donde x es para los cambios grandes e y para cambios 	menores. Esto perdmite que sea facil de identificar y enteder

	
