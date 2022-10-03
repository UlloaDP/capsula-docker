# [ING SW] Capsula Docker

# Introducción

En el desarrollo de software, comúnmente se parte con el diseño de la aplicación, sigue el desarrollo de la misma, luego se monta el código en un entorno de pruebas y finalmente se despliega a un servidor donde los usuarios pueden utilizar el programa. El uso de Docker, en particular el uso de **contenedores** tienen enfoque en esta última parte. Su objetivo es brindar un entorno único que no necesite reprogramarse cada vez que se monte la aplicación en un nuevo servidor, ofreciendo una mayor consistencia y modularidad.

En esta capsula, se les otorgará un pequeño tutorial donde veremos como instalar Docker, sus comandos básicos, como hacer un **dockerfile**, como iniciar un contenedor y como combinar contenedores en un **docker-compose**.

Si desean probar un ejemplo directamente asociado al proyecto que van a realizar, pueden pasar directamente al **paso 7: .** Por otro lado, si sienten que los contenidos son muy complejos, pueden ir paso a paso en este tutorial, aprovechando el ejemplo simple del **Paso 5: Ejemplo Práctico**[.](%5BING%20SW%5D%20Capsula%20Docker%20c602c89810c04aef8f89d6c0ff00434d.md)

# Requisitos

1. Tener una terminal o consola
2. Revisar si tienen puertos encendidos
3. Chequear si han instalado docker previamente
4. Recomendado: Probar el tutorial Docker 101 que ofrece el mismo sistema [https://www.docker.com/101-tutorial/](https://www.docker.com/101-tutorial/)

# 1. Instalación

Para empezar, descargaremos la aplicación “Docker Desktop” que facilitará el uso y despliegue de nuestros contenedores.

1. Ingresar a [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Escoger la versión adecuada para su sistema operativo (En este tutorial se utilizará Windows 10)

<aside>
💡 Puede aparecer esta advertencia tras la instalación:

![Untitled](%5BING%20SW%5D%20Capsula%20Docker%20c602c89810c04aef8f89d6c0ff00434d/Untitled.png)

Les pedirá actualizar el kernel de linux para la instalación de WSL 2(El subsistema de Windows para Linux). Para ello:

- Accedan al link que les deja el aviso.
- En el paso 4 habrá un archivo de descarga que contiene la actualización.
- Sigan los pasos del Wizard para instalar la actualización
- Una vez actualizado, reinicien Docker con el botón “Restart” del aviso
- Si se muestra algún tipo de error adicional, Cerrar Docker y abrirlo denuevo puede solucionar el problema
</aside>

# 2. Primer Proyecto

Al instalar Docker Desktop y abrirlo por primera vez, mostrará la posibilidad de realizar un tutorial de primeros pasos, para esta capsula seguiremos este tutorial.

1. En primer lugar importaremos un **repositorio de GitHub** en un **Contenedor**

```bash
docker run --name repo alpine/git clone [https://github.com/do](https://github.com/do)cker/getting-started.git
```

1. Luego, vamos a acceder al repositorio y construiremos una **Imagen de Docker,** 

```bash
cd getting-started
docker build -t docker101tutorial .
```

> Una imagen de Docker es un sistema de archivos privados para un contenedor, provee todos los archivos y código que el contenedor necesita.
> 
1. Ahora podemos iniciar un contenedor basado en la imagen que construimos en el paso anterior.

```bash
docker run -d -p 80:80 \ --name docker-tutorial docker101tutorial
```

1. El ultimo paso consiste en guardar y compartir la imagen que construimos en DockerHub, esto permite a otros usuarios de la red que puedan descargar y utilizar esta imagen. Para lo que se va a realizar en el curso no es necesario realizar este paso.

> Si desean seguir el tutorial de Docker, pueden abrir su puerto 80 y seguir la documentación que ofrece el sistema. Por otro lado si buscan seguir con las indicaciones para el proyecto pueden eliminar el contenedor en la sección **Containers.**
> 

# 3. Conceptos Básicos

Al seguir el tutorial básico aparecieron conceptos como **Contenedor e Imagen**, lo importante es entender que **un Contenedor es un entorno aislado de tu sistema con el fin de montar una Imagen.** 

> **¿Cual es la diferencia con una maquina virtual?:** Una VM representa una abstracción de una máquina (Del Hardware físico) por ende representa elementos como RAM, Disco duro, tarjetas de red, entre otros. Por ende requieren un sistema operativo completo, utilizando una gran cantidad de recursos y una menor velocidad de arranque.
> 

Por otro lado, un contenedor permite la misma aislación con un uso menor de los recursos, de esta forma se puede crear un gran número de contenedores que habiliten servicios.

## Arquitectura de un Docker

Docker funciona con una arquitectura RESTFUL, es decir, existe una comunicación entre cliente-servidor vía una RESTFUL api, el servidor, que llamaremos “Docker Engine” funciona como la base del sistema, el cual puede construir y correr contenedores de Docker (un contenedor no es más que un proceso del sistema).

## Comandos de Docker

En primer lugar vamos a chequear que tenemos docker en la terminal comprobando su versión.

```bash
> docker version
.
.
.
Server: Docker Desktop 4.12.0 (85629)
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
  Git commit:       a89b842
  Built:            Mon Jun  6 23:01:23 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.8
  GitCommit:        9cd3357b7fd7218e4aec3eae239db1f68a5a6ec6
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

Si poseen una versión menor, consideren actualizar su Docker 

# 4. Flujo de Desarrollo

**¿Como cambia mi forma de desarrollar usando docker?,**  de forma simple, vamos a “dockerizar” nuestra aplicación, no importa cual sea. Esto significa que agregaremos un pequeño archivo que permitirá a nuestra aplicación funcionar con Docker, este archivo se llama **DockerFile** y posee las instrucciones que necesita Docker para **empaquetar nuestra aplicación en una Imagen** que pueda ser **montada en un Contenedor.**

En general, una imagen contiene los siguientes elementos:

- Un sistema operativo base
- Un entorno de ejecución (Como Node o Python)
- Archivos de la aplicación
- Bibliotecas de terceros
- Variables de entornos

Entre otras características.

Así, en vez de cargar nuestra aplicación directamente en nuestra maquina local, indicamos a Docker que corra nuestra imagen en un contenedor.

La gracia ocurre al compartir esta imagen en un servidor externo. Al enviar la imagen a DockerHub o algún repositorio similar, y desde allí cargamos esta imagen en un servidor que sea compatible con Docker, mantendremos la misma configuración en todos los sitios, evitando tener que crear o modificar documentos por cada versión que se cambie de la aplicación.

# 5. Ejemplo Práctico

Ahora que conocemos los conceptos básicos, vamos a crear un simple y pequeño proyecto, al cual le vamos a asignar un **DockerFile** que cree una **Imagen** para luego montarla en un **Contenedor.**

### Creación de Aplicación

En su Editor de código favorito (En este tutorial utilizo VSCode) crearemos una carpeta llamada “mi-primer-docker” en la cual crearemos un archivo “app.js” que tendrá un console.log que diga “hola docker”

```jsx
console.log("Hola Docker");
```

### Dockerización

A esta aplicación la vamos a “Dockerizar”. Por lo general, si quisiéramos compartir esta aplicación con alguien más, enviaríamos el código, la otra persona tendría que instalar Node y ejecutar `node app.js` para hacerla funcionar. En cambio, vamos a crear un archivo dockerfile. que tendrá las siguientes instrucciones:

- Iniciar un sistema operativo
- Instalar Node
- Copiar los archivos del programa
- Correr el programa con `node app.js`

Vamos a crear el archivo Dockerfile en la misma carpeta, solo se necesita crear el archivo con el nombre para que VSCode detecte que es un archivo de ese tipo. Dentro de el, ingresaremos el siguiente código (Recomendado instalar las extensiones recomendadas para Docker):

```docker
FROM node:alpine
COPY . /app
WORKDIR /app
CMD node app.js 
```

Vamos a analizar lo que tiene el archivo:

- `FROM node:alpine` : al utilizar el comando FROM llamamos a un sistema operativo que nos otorgue los elementos mínimos para montar nuestro sistema, en este caso podemos llamar directamente a un entorno de ejecución Node montado en Alpine, que es una distribución de Linux bastante ligera.
- `COPY . /app`  : Este comando permite tomar los elementos que se encuentran en la carpeta actual y enviarlos a nuestra imagen en su carpeta /app, al utilizar el punto le decimos al sistema que tome todos los archivos que se encuentran en la carpeta actual.
- `WORKDIR /app` : Ayuda a que los comandos que vayamos a ejecutar se realicen en la carpeta /app
- `CMD node app.js` :  ejecuta el comando para iniciar la aplicación desde la carpeta definida en el comando anterior.

### Construcción de Imagen

Ahora que tenemos nuestro Dockerfile listo, vamos a construir nuestra imagen. Para ello, accedemos a nuestra terminal y ejecutamos:

```bash
docker build -t hola-docker .
```

- `-t` nos permite asociar un tag a nuestra imagen para identificarla más facilmente, en este caso se asignó el nombre “hola-docker”. Al estar ubicados en la misma carpeta donde está nuestro dockerfile, apuntamos la dirección con `.`
- 

```bash
PS D:\mi-primer-docker> docker build -t hola-docker .
[+] Building 11.6s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                 0.1s
 => => transferring dockerfile: 98B                                  0.0s
 => [internal] load .dockerignore                                    0.1s
 => => transferring context: 2B                                      0.0s
 => [internal] load metadata for docker.io/library/node:alpine       2.5s
 => [auth] library/node:pull token for registry-1.docker.io          0.0s
 => [internal] load build context                                    0.1s
 => => transferring context: 274B                                    0.0s
 => [1/3] FROM docker.io/library/node:alpine@sha256:7...             7.9s
...
 => [2/3] COPY . /app                                                0.8s
 => [3/3] WORKDIR /app                                               0.1s
 => exporting to image                                               0.1s
 => => exporting layers                                              0.1s
 => => writing image sha256:99f7711d058f2056...                      0.0s
 => => naming to docker.io/library/hola-docker                       0.0s
```

Ahora que creamos nuestra imagen, ¿Dónde se encuentra?, docker posee las imágenes que hemos creado y podemos verlas con el comando `docker image ls` 

```bash
REPOSITORY          TAG       IMAGE ID       CREATED             SIZE
hola-docker         latest    99f7711d058f   3 minutes ago       167MB
docker101tutorial   latest    87fd89ddffad   About an hour ago   28.9MB
```

También las podemos encontrar en la interfaz de Docker Desktop, en la sección “Imágenes”.

![Untitled](%5BING%20SW%5D%20Capsula%20Docker%20c602c89810c04aef8f89d6c0ff00434d/Untitled%201.png)

### Ejecución de Imagen

Ahora que construimos nuestra imagen, podemos correrla en cualquier dispositivo que tenga docker. Para ello utilizamos el comando `docker run hola-docker`, no es necesario señalar alguna dirección de archivo ya que en el Dockerfile señalamos la ruta que posee los elementos necesarios para correr la aplicación.

```bash
> docker run hola-docker
hola docker
```

La aplicación se ejecutó con éxito, al mostrar el texto en pantalla que está en nuestra aplicación.

---

<aside>
✅ De esta misma forma, podemos “Dockerizar” su proyecto de Ingeniería de Software y crear contenedores que ejecuten sus servidores de Backend y Frontend, incluso su Base de Datos!.

</aside>

### Monitoreo de Contenedores

Si nos interesa saber que contenedores están encendidos en nuestra máquina, podemos ejecutar el comando `docker ps -a` o abrir la sección “Containers” en Docker Desktop

```bash
docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED        STATUS     PORTS
6ac025d363c3   hola-docker   "docker-entrypoint.s…"   14 minutes ago Exited (0)      
```

![Untitled](%5BING%20SW%5D%20Capsula%20Docker%20c602c89810c04aef8f89d6c0ff00434d/Untitled%202.png)

Como la aplicación solo era un `console.log`, se inició y apagó inmediatamente, por lo que no quedó encendida en nuestra lista de contenedores. Por otro lado, las aplicaciones o servidores que van a crear en su proyecto deben quedar encendidas para que el servidor funcione. Es recomendable chequear constantemente el estado de los contenedores para ver si todo está en orden.

# 6. Conceptos Básicos de Linux

Para utilizar Docker es necesario conocer los comandos básicos de Linux, ya que fue construido en base a este tipo de instrucciones. Por otro lado será positivo conocer algunos tipos de distribuciones para levantar un proyecto en Docker.

## Distribuciones

Dentro de las más utilizadas se encuentran:

- Ubuntu
- Debian
- Alpine
- Fedora
- CentOS

En general cualquiera debería funcionar para un servidor básico, poseen ligeras diferencias que las distinguen ante otras. En este caso, para el proyecto de Ingeniería de software, se utilizará Alpine o Ubuntu.

## Comandos Básicos

Para moverse en un entorno linux, es necesario conocer como mínimo estos comandos:

- `ls` : Muestra en una lista todos los archivos en la carpeta actual
- `cd` : Permite navegar en el sistema, si no se escribe nada, redirige a la carpeta HOME
- `cd ..` : Permite salir de la carpeta actual, o mejor dicho, subir de nivel
- `cat > filename` : Crea un nuevo archivo con el nombre ‘filename’
- `cat filename` : Muestra el contenido del archivo ‘filename’
- `mv file "new file path”` : Mueve el archivo ‘filename’ a otra dirección
- `sudo` : Otorga permisos de administrador para ejecutar alguna acción
- `rm filename`: elimina el archivo ‘filename’
- `man` : Otorga información sobre algún comando

Para más información del resto de comandos que ofrece linux, pueden revisar esta referencia

[Linux Command Cheat Sheet](https://www.guru99.com/linux-commands-cheat-sheet.html)

# 7. Interactuar con un contenedor

En el ejemplo anterior, vimos como Dockerizar una aplicación sencilla que solo mostraba un mensaje en pantalla. Ahora vamos a poner en práctica estos conceptos creando un proyecto general. Una aplicación de software que utiliza microservicios necesita como mínimo:

- Un servidor que maneje el Front-End
- Un servidor que maneje el Back-End
- Una Base de Datos

Al Dockerizar estos tres elementos tendríamos un contenedor para cada servidor respectivamente. Por ende necesitaríamos tres archivos dockerfile que representen la imagen respectiva.

Si el proyecto fuera más grande y con más tipos de servidores podría tomar mucho tiempo organizar todos estos archivos. Aparte se tendrían que realizar ajustes de conexión desde fuera y podría generar problemas en la aplicación. 

Para ello, Docker ofrece la posibilidad de realizar un **docker-compose,** el cual permite en un solo archivo identificar las imágenes a crear, seleccionar los volúmenes de información y crear una red para todas las imágenes que van a ser levantadas.

### Ejemplo de Proyecto

Queremos Dockerizar un proyecto que está organizado de la siguiente forma:

```bash
mi-proyecto/frontend
	|-Archivos de Front-end
	|-dockerfile

mi-proyecto/backend
	|- Archivos de Back-end
	|- dockerfile

```

Como se puede apreciar, tenemos dos servidores y nos gustaría conectarlos y poder ejecutarlos con un solo comando. Para ello creamos un archivo `docker-compose.yaml` en la carpeta mi-proyecto que contenga lo siguiente

```yaml
version: "3.9"

services:
  frontend:
    build:
			context: ./frontend
			dockerfile: dockerfile
    ports:
      - 3000:3000
    environment:
			- HOST=0.0.0.0
	    - ...
	backend:
		build:
      context: ./backend
      dockerfile: Dockerfile
		ports:
			- 4000:80
		
  oracle:
    image: gvenzl/oracle-xe:21-slim        
    container_name: oracledatabase  
    volumes:      
      - ./oracle-volume:/opt/oracle/oradata
    ports:    
      - 1521:1521
    environment:      
      ORACLE_PASSWORD : root
      ORACLE_DATABASE : eis
      APP_USER: dbuser
      APP_USER_PASSWORD: dbuser
	volumes:
			
```

En este archivo, se declaran los servicios que van a ser montados en el docker-compose, así se crearon los servicios **frontend, backend y oracle.** Cada servicio describe los siguientes elementos

- `build:` : Contiene los atributos context y dockerfile, donde se señala en que carpeta se va a llamar el dockerfile que representa el servicio.
- `ports:` : Representa el puerto en que el contenedor va a funcionar, siendo el primer puerto el externo y el segundo el interno.
- `environment:` : Aquí se establecen las variables de entorno del sistema, las cuales facilitan integrar información global que debe ser resguardada con cuidado.

# Referencias

[https://www.youtube.com/watch?v=pTFZFxd4hOI](https://www.youtube.com/watch?v=pTFZFxd4hOI)

[https://www.youtube.com/watch?v=8fi7uSYlOdc](https://www.youtube.com/watch?v=8fi7uSYlOdc)

[https://www.youtube.com/watch?v=6idFknRIOp4](https://www.youtube.com/watch?v=6idFknRIOp4)

[https://www.youtube.com/watch?v=CV_Uf3Dq-EU](https://www.youtube.com/watch?v=CV_Uf3Dq-EU)