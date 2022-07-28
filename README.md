# Comandos generales de docker

```bash
docker images - ver imagenes disponibles que yo tengo
docker pull [nombre imagen] - para descargar una imagen
docker image rm [nombre imagen] - para eliminar una imagen
docker create [nombre imagen base] - para crear un contenedor a partir de una imagen.
docker star [id del contenedor] / [nombre contenedor]
docker ps - para ver los contenedores que estan levantados
docker stop [id del contenedor] / [nombre contenedor] - para detener el contenedor que se esta ejecutando
docker ps -a - ver todos los contenedores en nuestro sistema (corriendo y no)
docker create --name [nombre contenedor] [nombre imagen base]
docker -p27017 (puerto de mi maquina fisica):27017(puerto del contenedor que vamos a mapear con nuestra maquina) --name [nombre] [nombre imagen]
si no le pongo el puerto con el -p me va a poner unos por defecto.
docker logs [nombre contenedor] - ver todos los logs de ese contenedor hasta el momento y me devuelve a terminal
docker logs --follow [nombre contenedor] - veo los logs y me quedo esperando a ver si aparecen mas logs
docker run [nombre imagen] - ver si existe la imagen, si no la encuentra la descarga, crea el contenedor y lo inicia (forma mas rapida) -va mostrando el log
docker run -d [nombre imagen] - no muetra logs mientra descarga
docker run --name [nombre] -p[puertoPC]:[puertoCONTENEDOR] -d [nombre imagen] - esta seria la forma que engloba todo lo anterior
docker create -p[puerto]:[puerto] --name [nombre] -e [aca van las variables de entorno que necesite el contenedor para quedar configurado] [nombre imagen]
las variables de entorno para configurar las puedo revisar en docker hub.
```
Dockerfile = se usa para crear imagenes con varias configuraciones
ejemplo:
FROM node:18 (etiqueta/version)

RUN mkdir -p /home/app (donde voy a tener codigo fuente de mi app dentro del contenedor - no es mi PC)

COPY . /home/app (ruta de mi ssoo anfritrion para tomar y colocar el codigo en el contenedor)

EXPOSE 3000 (puerto donde quiero exploner la app para que otros contenedores o app se puedan conectar a el)
CMD ["node", "/home/app/index.js] (comando para poder ejecutar y hacer correr la app. se pone el comando + argumentos) 


para que varios contenedores se puedan comunicar entre si, deben estar dentro de la misma red de contenedores.

docker network ls - ver redes disponibles
docker network create [nombre red] - crear una red de contenedores
docker network rm [nombre red] - eliminar una red
docker build -t [nombre app]:[le podemos poner una etiqueta] [ruta donde esta mi Dockerfile (si es . significa que es ahi donde estoy parado] - para crear un contenedor a partir de un Dockerfile
docker create -p --name --network [nombre de la red a la que pertenece] -e [nombre de imagen]

#Usar angular con contenedores

* EN VS CODE instalar las extensiones de remote container y docker.

* Crear carpeta donde guardamos el proyecto en nuestra pc
* Creamos dentro una carpeta .devcontainer y adentro un devcontainer.json y Dockerfile


json:
```json
{
    "name": "dockerNode",
    "dockerFile": "Dockerfile",
    "appPort": 4200
}
```

* appPort = puerto de la app nombre y que usa el docker file

1. Docker File:
```Dockerfile
FROM node

RUN npm install -g @angular/cli

EXPOSE 4200
```
* luego que hicimos los archivos, salir y abrir el proyecto para que Vs code arranque el contenedor solo.


* despues de eso, abrimos una terminal y ya estamos adentro del contenedor. 
* en este punto tenemos el entorno listo.
* crear aplicacion de angular una vez con ng new [nombreApp] - se crea en nuestra carpeta local.
* Automanticamente el contenedor mappea nuesta carpeta con el codigo en el contenedor y el puerto 4200 tambien.

Cuando cierre docker desktop no se detiene el contenedor
hay que detenerlo a mano con la app.
* para empezar a desarrollar basta con entrar a vs code y el lo levanta solo