1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?
El frontend solo se encarga de mostrar la web al usuario. Cuando necesita datos, se los pide al backend, y es el backend quien habla con la base de datos. Es como un camarero: el cliente le pide al camarero, y el camarero va a la cocina; el cliente no entra a la cocina.

2. ¿Por qué el backend pertenece a dos redes?
Porque es el intermediario. Tiene que hablar con el frontend (red frontend) y con la base de datos (red datos). Al estar en las dos, puede pasar la información de un lado a otro.

3. ¿Qué servicio necesita publicar un puerto en el anfitrión?
El frontend, porque es lo único que el usuario abre desde el navegador (localhost:8080). El backend y la base de datos solo se usan desde dentro, así que no hace falta que sean accesibles desde fuera.

4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
Si dos servicios internos no se hablan, el problema está en cómo están conectados (redes, nombres, configuración), y abrir un puerto no lo arregla. Además, dejar la base de datos accesible desde fuera es un riesgo de seguridad, porque cualquiera podría intentar entrar a los datos.

5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
Todo queda escrito en un solo archivo y se levanta con un único comando (docker compose up -d). Manualmente habría que escribir muchos comandos largos, crear redes y volúmenes a mano y recordar el orden. Con Compose es más rápido, hay menos errores y se puede repetir igual en otro ordenador.

6. ¿Por qué hemos dividido la arquitectura en dos redes?
Para separar y proteger. El frontend, que es la parte más expuesta, no tiene ningún camino hasta la base de datos. Si alguien lograra entrar al frontend, no podría llegar directamente a los datos. Cada servicio solo ve a quien realmente necesita.

7. ¿Por qué el backend utiliza `db` como dirección de PostgreSQL?
Porque db es el nombre del servicio en el Compose, y Docker se encarga de traducir ese nombre a la dirección real del contenedor. Se usa el nombre porque las direcciones IP pueden cambiar cada vez que se recrea el contenedor, mientras que el nombre siempre es el mismo. Y no se usa localhost porque dentro del contenedor del backend, localhost es el propio backend, no la base de datos.

8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
Detenerlo (o hacer down) apaga los contenedores pero los datos se conservan, porque están guardados en el volumen. Eliminar los volúmenes (down -v) borra además esos datos para siempre. Es la diferencia entre apagar el ordenador y formatear el disco.

9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL
Se perderían todas las incidencias registradas: el historial, el estado en que estaba cada una y cualquier información guardada. La aplicación seguiría arrancando, pero vacía. Para la empresa significaría perder el trabajo hecho y no poder hacer seguimiento de los problemas, por eso es importante hacer copias de seguridad.
