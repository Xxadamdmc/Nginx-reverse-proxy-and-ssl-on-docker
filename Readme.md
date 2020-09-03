# Introduccón

En este post vamos a ver como crear un servidor proxy inverso y la automatizar la creacion de los certificdos ssl seguros 100% usando letsencript  en docker y NGINX-proxy creando un valanceador de cargas redireccionando las peticiones http/https entrantes a su servicio web correspondiente por nombe de dominio.

### Como funciona:

Tenemos varios contenedores corriendo, un servidor nginx cada uno  (contenedor1) www.web1.com, (contenedor2) www.web2.com,(contenedor3) www.web3.com
NGINX-Proxy tiene acceso a la api de docker, asi cada vez que es creado o eliminado un contender actualiza la tabla de contenedores.
En cada contenedor usaremos las variables de entorno VIRTUAL_HOST y LETSENCRIPT_HOST:

- VIRTUAL_HOST: Esta variable la usa nginx-proxy para redirigir el trafico al contenedor segun el nombre de dominio de la peticion

### Ejemplo: 

tenemos expuesto el servidor nginx-proxy en el puerto 80, usaremos el nombre de dominio www.web1.com, el contenedor 1 (con la la variable VIRTUAL_HOST=wwww.web1.com).
cada vez que nuestro nginx-proxy reciba la peticion http/https wwww.web1.com redirigira el trafico al contenedor 1.

- LETSENCRIPT_HOST: el contenedor letsencript tambien tiene acceso a la api de docker, con la cual puede ver que contenedores tienen la variable LETSENCRIPT_HOST y genera  automaticamente certificados ssl y los valida con letsencript para que sean 100% seguros (de confianza), asi cada 60 minutos (1 hora) verifica que los certificados esten bien y si estan cerca de caducar los renova automaticamente.

> WARNING : **Para este paso hace falta tener el nombre de dominion externalizado (servidor DNS) apuntando a nuestra ip publica, si no no podra validar los certificados**

<img src="./diagrama.png">

# Ejecturar
 > CMD : **docker-compose up -d**
