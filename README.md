# Docker
#### Gómez Aldrete Luis Ignacio 
#### 216588253
#
Para esta actividad se va a realizar un ejemplo básico con *Docker*. Como no tenía *Docker* instalado, me fui a su [página](https://www.docker.com/) para descargar *Docker Desktop*.

![image](https://user-images.githubusercontent.com/80866790/226127028-c9b49747-0bdc-4562-a9e1-7255a4fb0c3e.png)

Después de configurar el programa y reiniciar el equipo para finalizar la instalación, opté por tomar el tutorial que el mismo *Docker Desktop* provee. Para empezar el tutorial simplemente tuve que escribir el siguiente comando en la terminal:

```cmd
docker run -d -p 80:80 docker/getting-started
```

Y posteriormente me tuve que dirigir a la dirección de *localhost* para empezar el tutorial.

![image](https://user-images.githubusercontent.com/80866790/226129252-50d8b855-8190-429e-8c92-0d635ff90b1c.png)

Para el tutorial de *Docker*, se me proporcinó el codigo de una aplicación de una lista de tareas. La misma página de *Docker* provee un zip con la aplicación. El zip contiene los siguientes archivos.

![image](https://user-images.githubusercontent.com/80866790/226134338-6a016c31-4916-46cf-a0b3-aba26ded1a41.png)

Para continuar el desarrollo se tiene que crear un archivo *Dockerfile* que se encarga de crear la imagen del contenedor. Con el siguiente código:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```
![image](https://user-images.githubusercontent.com/80866790/226135377-b586bb29-482f-4815-97c0-4fc7a3ca37c7.png)

Después se construye la imagen del contenedor con el siguiente comando en una terminal dentro del directorio de la aplicación:

```cmd
docker build -t getting-started .
```

Ya con la imagen construida, simplemente hay que correr el contenedor con el siguiente comando: 

```cmd
docker run -dp 3000:3000 getting-started
```

Y ya que esta corriendo el contenedor, simplemente hay que ir al *localhost* en el puerto 3000 para ver la aplicación de la lista.

![image](https://user-images.githubusercontent.com/80866790/226136722-13cd52b8-2721-4231-b22c-4cad60e47a69.png)

Inclusive es posible ver el contenedor ejecutandose en *Docker Desktop*.

![image](https://user-images.githubusercontent.com/80866790/226136768-d224f17b-7822-4791-be75-4c1081a699e2.png)

Para el tutorial, se modifica una pequeña parte del código de la aplicación, donde se cambia el mensaje que aparece cuando no hay elementos en la lista, este pasa de ser ```No items yet! Add one above!``` a ser ```You have no todo items yet! Add one above!```. Y para hacer efectivo este cambio, se tiene que volver a contruir la imagen del contenedor, pero además se tiene que detener y remover el contenedor anterior. Esto se puede hacer mediante los comandos: 

```cmd 
docker stop <container-id>
docker rm <container-id>
```

Solo que se reemplazan los ```<container-id>``` con el id del contenedor que se quiere detener y remover, en este caso el id es: ```2cbea574a3855cb709eba702cde2336528b67547b4a8f0f99f3726ec2f41292b```.

Aunque también es posible remover el contenedor desde *Docker Desktop*, ahí solo hay que encontrar el contenedor a remover, darle click al botón de basura y confirmar su eliminación. 

![image](https://user-images.githubusercontent.com/80866790/226137545-42a155b2-d68e-4418-b928-a070fe178024.png)

Ahora solo se vuelve a ejecutar la apliación actualizada y se podrán ver los cambios al actualizar la página del navegador.

![image](https://user-images.githubusercontent.com/80866790/226137939-e519e077-05f0-457f-95a6-cde68e40efa6.png)

Como última modificación a realizar en la aplicación, se va a configurar la manera de que cada que se lance el contenedor, persistan las tareas que se van agregando a la lista. Esto se hace creando un volumen que permite conectar paths del sistema de archivos con el contenedor. Esto quiere decir que si un directorio de un contenedor es montado, los cambios realizados en ese directorio son reflejados en la máquina host.

Para realizar esto, primero se crea el volumen con el comando: 

```cmd
docker volume create todo-db
```

Y después se detiene el contenedor actual de la lista, para empezarlo de nuevo, pero ahora con el volumen. 

```cmd
docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
```

![image](https://user-images.githubusercontent.com/80866790/226140873-b214232c-c642-4f0a-a7d7-087cad4c535f.png)

Ahora si detego y remuevo el contenedor y posteriormente vuelvo a empezar el contenedor, las tareas de la lista seguirán ahí.

![image](https://user-images.githubusercontent.com/80866790/226141055-4019770f-0545-4ee8-8b7f-755f5d83c986.png)
