# Introducción a la contenerización

![bandera](./images/banner1.jpeg)

**Última actualización:** marzo de 2024

**Duración:** 45 minutos

¿Necesita ayuda? Póngase en contacto con **Kevin Postreich, Yi Tang**

[Regresar a la página principal de laboratorios](../README.md)

## Fondo

Si está esperando un laboratorio sobre `containers and docker` , está en el lugar correcto. Este laboratorio le presentará los conceptos básicos de la contenedorización, incluidos:

- ¿Qué son los contenedores y las imágenes de contenedores?
- Cómo iniciar, detener y retirar contenedores.
- Cómo crear imágenes de contenedores
- Cómo versionar imágenes de contenedores

<a name="Prerequisites"> </a>

## Prerrequisitos

- Tienes instalado `podman` o `docker` . Solo `docker` está instalado para este laboratorio.
- Tienes acceso a internet.

<a name="What_is_Container"> </a>

## ¿Qué es un contenedor?

En comparación con las máquinas virtuales, los contenedores admiten la virtualización a nivel de proceso. Piense en ellos como procesos virtuales. La abstracción de aislamiento que proporciona el sistema operativo hace que el proceso piense que se está ejecutando en su propia máquina virtual. Como procesos, los contenedores se pueden crear, iniciar y detener mucho más rápido que las máquinas virtuales.

Todo lo que necesita para ejecutar su aplicación, desde el sistema operativo en adelante, se almacena en un archivo especial llamado imagen de contenedor.
 Las imágenes de contenedor son autónomas y portátiles. Puede ejecutar una o más instancias en cualquier lugar y no tiene que preocuparse por la falta de requisitos previos, ya que todos ellos se almacenan en la imagen.

Las imágenes de contenedores se crean mediante herramientas como `docker` o `podman` . Las imágenes existentes se alojan en registros de contenedores. Por ejemplo, docker hub, o registration.access.redhat.com, o su propio registro interno.

Si necesita más información sobre los contenedores: https://www.docker.com/resources/what-container

<a name="Login_VM"> </a>

## Accediendo al entorno

Si está realizando este laboratorio como parte de un taller dirigido por un instructor (virtual o presencial), ya se le ha proporcionado un entorno. El instructor le proporcionará los detalles para acceder al entorno del laboratorio.

De lo contrario, deberá reservar un entorno para el laboratorio. Puede obtener uno aquí. Siga las instrucciones en pantalla para la opción “ **Reservar ahora** ”.

[https://techzone.ibm.com/my/reservations/create/65eb652a747d7a00108fb5db](https://techzone.ibm.com/my/reservations/create/65eb652a747d7a00108fb5db)

El entorno de laboratorio contiene seis (6) máquinas virtuales Linux.

![](./images/env-list.png)

<br>

1. Acceda al entorno de laboratorio desde su navegador web.

    Se configura un `Published Service` para proporcionar acceso a la máquina virtual **`Workstation`** a través de la interfaz noVNC para el entorno de laboratorio.

    a. Cuando se haya aprovisionado el entorno de demostración, haga clic en el **`environment tile`** para abrir su vista de detalles.

    b. Haga clic en el enlace **`Published Service`** que mostrará una **lista del directorio.**

    c. Haga clic en el enlace **`vnc.html`** para abrir el entorno de laboratorio a través de la interfaz **noVNC** .

    ![](./images/vnc-link.png)

    d. Haga clic en el botón **`Connect`**

    ![](./images/vnc-connect.png)

    e. Ingrese la contraseña como: **`passw0rd`** . Luego haga clic en el botón **`Send Credentials`** para acceder al entorno de laboratorio.

    > Nota: Ese es un cero numérico en passw0rd

    ![](./images/vnc-password.png)

     <br>
    

2. Si se le solicita iniciar sesión en la máquina virtual de la "estación de trabajo", utilice las credenciales a continuación:

    Las credenciales de inicio de sesión para la **estación de trabajo** VM son:

    - ID de usuario: **techzone**

    - Contraseña: **IBMDem0s!**

    > Nota: Ese es un cero numérico en la contraseña.

     <br>


    ![pantalla de vm de estudiante](./images/techzone-user-pw.png)

     <br>
    

## Consejos para trabajar en el entorno de laboratorio

1. Puede cambiar el tamaño del área visible utilizando las opciones **de Configuración de noVNC** para cambiar el tamaño del escritorio virtual para que se ajuste a su pantalla.

    a. Desde la máquina virtual del entorno, haga clic en el icono **giratorio** en el panel de control noNC para abrir el menú.

    ![ajustar a la ventana](./images/z-twisty.png)

    b. Para aumentar el área visible, haga clic en `Settings > Scaling Mode` y configure el valor en `Remote Resizing`

    ![ajustar a la ventana](./images/z-remote-resize.png)

2. Puede copiar/pegar texto de la guía de laboratorio en el entorno de laboratorio utilizando el portapapeles en el visor noVNC.

    a. Copie el texto de la guía de laboratorio que desea pegar en el entorno de laboratorio.

    b. Haga clic en el icono **del Portapapeles** y **pegue** el texto en el portapapeles de noVNC

    ![ajustar a la ventana](./images/paste.png)

    c. Pegue el texto en la máquina virtual, por ejemplo en una ventana de terminal, una ventana del navegador, etc.

    d. Haga clic en el icono **del portapapeles** nuevamente para cerrarlo.

3. Como alternativa a la opción Copiar/Pegar de noVNC, puede considerar abrir la guía de laboratorio en un navegador web dentro de la máquina virtual. Con este método, puede copiar y pegar fácilmente texto de la guía de laboratorio sin tener que usar el portapapeles de noVNC.

     <br>
    

<a name="Check_Environment"> </a>

## Comprueba tu entorno y clona el proyecto de GitHub del taller

1. Abra una ventana de terminal desde su VM.

    ![Terminal](images/checkenv1.png)

     <br>
    

2. Lista de versiones de Docker:

    ```
    docker --version
    ```

    Ejemplo de salida:

    ```
     Docker version 23.0.1, build a5ee5b1
    ```

    - Para obtener más información sobre la línea de comandos de Docker: [https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)

     <br>
    

3. Desde la ventana de terminal en la VM, clone el repositorio de Github en su directorio local.

    > El repositorio de Github contiene los artefactos necesarios para completar este laboratorio.

    ```
     cd /home/techzone
     
     git clone https://github.com/IBMTechSales/appmod-pot-labfiles.git
    ```

4. Cambie el directorio a la copia local clonada del repositorio git. `/home/techzone/appmod-pot-labfiles/labs/IntroContainers`

    ```
     cd /home/techzone/appmod-pot-labfiles/labs/IntroContainers
    ```

<a name="Run_Prebuilt"> </a>

## Ejecutar una imagen prediseñada

En esta sección del laboratorio, trabajará con una imagen de contenedor prediseñada de DockerHub público.

1. Las imágenes de contenedor deben estar disponibles localmente antes de poder ejecutarlas. Para enumerar las imágenes locales disponibles:

    ```
    docker images
    ```

    **Nota:** Es posible que vea algunas imágenes que ya existen en la VM y que se utilizan para diferentes laboratorios.

    ```
     REPOSITORY   TAG   IMAGE ID   CREATED   SIZE

     default-route-openshift-image-registry.apps.ocp.ibm.edu/db2/db2_demo_data   latest    ...

     icr.io/appcafe/transformation-advisor-ui  3.8.1  ...

     icr.io/appcafe/transformation-advisor-server 3.8.1  ...

     icr.io/appcafe/transformation-advisor-db  3.8.1  ...

     icr.io/appcafe/transformation-advisor-neo4j  3.8.1  ...

     ibmoms/db2express-c  latest  ...
    ```

2. Las imágenes se alojan en registros de contenedores. El registro de contenedores predeterminado para Docker es Docker Hub, ubicado en https://hub.docker.com. Extraigamos una imagen de prueba de Docker Hub:

    ```
    docker pull openshift/hello-openshift
    ```

    Y el resultado:

    ```
      Using default tag: latest
      latest: Pulling from openshift/hello-openshift
      4f4fb700ef54: Pull complete
      8b32988996c5: Pull complete
      Digest: sha256:aaea76ff622d2f8bcb32e538e7b3cd0ef6d291953f3e7c9f556c1ba5baf47e2e
      Status: Downloaded newer image for openshift/hello-openshift:latest
      docker.io/openshift/hello-openshift:latest
    ```

3. Vuelve a enumerar las imágenes locales disponibles:

    ```
    docker images | grep -B1 hello
    ```

    La imagen **hello-openshift** ahora está listada

    ```
    REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
    openshift/hello-openshift   latest              7af3297a3fb4        2 years ago         6.09MB
    ```

4. Inspeccione los metadatos de la imagen:

    ```
    docker inspect openshift/hello-openshift
    ```

    **Nota** :

    - Expone dos puertos: **8080** y **8888**

    - Se ejecuta como usuario **1001**

    - El ejecutable del punto de entrada es **/hello-openshift**

        ```
          [
          	{
          		"Id": "sha256:7af3297a3fb4487b740ed6798163f618e6eddea1ee5fa0ba340329fcae31c8f6",
          		"RepoTags": [
          			"openshift/hello-openshift:latest"
          		],
          		"RepoDigests": [
          			"openshift/hello-openshift@sha256:aaea76ff622d2f8bcb32e538e7b3cd0ef6d291953f3e7c9f556c1ba5baf47e2e"
          		],
          	
          		...
          	
          		"Config": {
          			"User": "1001",
          			...
          		
          			"ExposedPorts": {
          				"8080/tcp": {},
          				"8888/tcp": {}
          			},
          		   "Env": [
          				"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
          			],
          			...
          		
          			"Entrypoint": [
          				"/hello-openshift"
          			]
          			...
          	}
          ]
        ```

        ![hola-openshift-detalle](images/hello-openshift-details.png)

5. Ejecute la imagen en un contenedor: observe los puertos expuestos (8083 y 8888) en el comando **docker run** .

    ```
    docker run --name hello1 -d -p 8083:8080 -p 8888:8888 openshift/hello-openshift
    ```

    Tenga en cuenta que:

    - La opción `--name` le da un nombre al contenedor.
    - La opción `-d` ejecuta el comando en segundo plano como un demonio.
    - La opción `-p` asigna el puerto del host al puerto del contenedor. A través de redes virtuales, el puerto dentro del contenedor siempre es el mismo para todas las instancias en ejecución. Sin embargo, para admitir varias instancias en ejecución simultáneas, el puerto real del host debe ser diferente para cada instancia. Cuando inicia el contenedor, puede asignar un nuevo puerto en el host de forma dinámica.
    - La salida del comando es el ID del contenedor en ejecución.
    - Si el contenedor se inicia correctamente, se ejecuta el archivo ejecutable especificado por el `Entrypoint` en los metadatos. En nuestro ejemplo, es `/hello-openshift` .

     <br>
    

6. Acceda a la aplicación en el contenedor.

    a. Abra el navegador web Firefox desde dentro de la máquina virtual.

    ![Firefox](images/runprebuilt3.png)

    b. Vaya a la URL `http://localhost:8083`

    c. Pruebe también el puerto **8888**

    ![Hola openshift](images/runprebuilt1.png)

7. Ejecute otra instancia de la misma imagen. Tenga en cuenta que a esta nueva instancia se le asignan los nuevos números de puerto 8084 y 8889 en el host. Esto es para que no entren en conflicto con los puertos 8083 y 8888 ya asignados a la primera instancia.

    ```
    docker run --name hello2 -d -p 8084:8080 -p 8889:8888 openshift/hello-openshift
    ```

    **Pregunta:** ¿Cómo se compara esto con el tiempo que lleva iniciar una nueva máquina virtual?

     <br>
    

8. Acceda a la aplicación en el nuevo contenedor de la misma manera.

    a. Regrese al navegador web Firefox pero acceda a la URL `http://localhost:8084`

    b. Pruebe también el puerto **8889**

    ![Hola openshift](images/runprebuilt2.png)

9. Utilice el comando `docker ps` para verificar que haya dos contenedores ejecutándose en el mismo host:

    ```
     docker ps | grep -B1 hello
    ```

    > Nota: El "ID DEL CONTENEDOR" puede ser diferente al que se muestra en la ilustración. No hay problema.

    ```
    CONTAINER ID        IMAGE                       COMMAND              CREATED              STATUS              PORTS                                            NAMES
    5a62f8527b44        openshift/hello-openshift   "/hello-openshift"   About a minute ago   Up About a minute   0.0.0.0:8084->8080/tcp, 0.0.0.0:8889->8888/tcp   hello2
    c9d49aaa01b7        openshift/hello-openshift   "/hello-openshift"   4 minutes ago        Up 4 minutes        0.0.0.0:8083->8080/tcp, 0.0.0.0:8888->8888/tcp   hello1
    ```

10. Ver los registros:

```
docker logs hello1
```

Y el resultado:

```
 serving on 8888
 serving on 8080
```

1. Ver los registros del segundo contenedor:

```
docker logs hello2
```

Y el resultado:

```
  serving on 8888
  serving on 8080
```

**Nota:** dentro del contenedor, cada instancia se comporta como si estuviera ejecutándose en su propio entorno virtual y tuviera abiertos los mismos puertos. Fuera del contenedor, se abren distintos puertos.

  <br>


1. Para exportar el sistema de archivos de un contenedor en ejecución:

```
docker export hello1 > hello1.tar
```

> **Nota:** Puede utilizar el comando docker export para exportar un contenedor a otro sistema como un archivo tar de imagen. También debe exportar por separado los volúmenes de datos que utiliza el contenedor.

1. Enumere los archivos en el sistema de archivos:

```
tar -tvf hello1.tar
```

**Tenga en cuenta** que esta es una imagen muy pequeña.

```
    -rwxr-xr-x 0/0               0 2020-04-29 16:48 .dockerenv
    drwxr-xr-x 0/0               0 2020-04-29 16:48 dev/
    -rwxr-xr-x 0/0               0 2020-04-29 16:48 dev/console
    drwxr-xr-x 0/0               0 2020-04-29 16:48 dev/pts/
    drwxr-xr-x 0/0               0 2020-04-29 16:48 dev/shm/
    drwxr-xr-x 0/0               0 2020-04-29 16:48 etc/
    -rwxr-xr-x 0/0               0 2020-04-29 16:48 etc/hostname
    -rwxr-xr-x 0/0               0 2020-04-29 16:48 etc/hosts
    lrwxrwxrwx 0/0               0 2020-04-29 16:48 etc/mtab -> /proc/mounts
    -rwxr-xr-x 0/0               0 2020-04-29 16:48 etc/resolv.conf
    -rwxr-xr-x 0/0         6089990 2018-04-18 10:22 hello-openshift
    drwxr-xr-x 0/0               0 2020-04-29 16:48 proc/
    drwxr-xr-x 0/0               0 2020-04-29 16:48 sys/
```

1. Ejecutar comandos en el contenedor en ejecución.

    Puedes acceder al contenedor en ejecución para ejecutar otro comando.

    El caso de uso típico es ejecutar un comando de shell, de modo que pueda utilizar el shell para navegar dentro del contenedor y ejecutar otros comandos. Sin embargo, nuestra imagen es pequeña y no hay un shell integrado.

    Para el propósito de este laboratorio, ejecutaremos el comando para invocar el ejecutable **hello-openshift** que acabamos de extraer del archivo tar:

    > **Nota:** ejecutar el siguiente comando generará un error, porque ya hay otra copia ejecutándose en segundo plano que está vinculada a los puertos 8080 y 8888:

    ```
    docker exec -ti hello1 /hello-openshift .
    ```

    ```
    serving on 8888
    serving on 8080
    panic: ListenAndServe: listen tcp :8888: bind: address already in use
    ...
    ```

2. Detenga los contenedores que tiene en ejecución:

    ```
    docker stop hello1
    docker stop hello2
    ```

3. Lista de contenedores en ejecución:

    ```
    docker ps | grep -B1 hello
    ```

    No debería haber ningún contenedor en ejecución enumerado que coincida con el nombre "hola"

    ```
    CONTAINER ID        IMAGE                       COMMAND              CREATED

    ```

4. Enumere los contenedores hello-openshift, incluidos los contenedores detenidos:

    ```
    docker ps -a | grep -B1 hello
    ```

    Deberías ver los dos contenedores enumerados, aunque no estén en ejecución; el ESTADO es **Salido**

    ```
    CONTAINER ID        IMAGE                       COMMAND              CREATED             STATUS                      PORTS               NAMES
    5a62f8527b44        openshift/hello-openshift   "/hello-openshift"   28 minutes ago      Exited (2) 28 seconds ago                       hello2
    c9d49aaa01b7        openshift/hello-openshift   "/hello-openshift"   31 minutes ago      Exited (2) 32 seconds ago                       hello1
    ```

5. Reiniciar un contenedor detenido:

    ```
    docker restart hello1
    ```

6. Lista de contenedores en ejecución:

    ```
    docker ps | grep -B1 hello
    ```

    El contenedor hello-openshift ahora está ejecutándose nuevamente.

    ```
    CONTAINER ID        IMAGE                       COMMAND              CREATED             STATUS              PORTS                                            NAMES
    c9d49aaa01b7        openshift/hello-openshift   "/hello-openshift"   33 minutes ago      Up 8 seconds        0.0.0.0:8083->8080/tcp, 0.0.0.0:8888->8888/tcp   hello1
    ```

7. Detener el contenedor:

    ```
    docker stop hello1
    ```

8. Elimine los contenedores detenidos y observe que se han eliminado los contenedores hello1 y hello2:

    ```
    docker rm hello1
    docker rm hello2
    docker ps -a | grep -B1 hello
    ```

9. Eliminar la imagen del caché local:

    a. Ver imágenes actuales:

    ```
    docker images | grep hello
    ```

    Ejemplo de salida:

    ```
    REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
    openshift/hello-openshift   latest              7af3297a3fb4        2 years ago         6.09MB
    ```

    b. Eliminar la imagen:

    ```
    docker rmi openshift/hello-openshift
    ```

    Ejemplo de salida:

    ```
    Untagged: openshift/hello-openshift:latest
    Untagged: openshift/hello-openshift@sha256:aaea76ff622d2f8bcb32e538e7b3cd0ef6d291953f3e7c9f556c1ba5baf47e2e
    Deleted: sha256:7af3297a3fb4487b740ed6798163f618e6eddea1ee5fa0ba340329fcae31c8f6
    Deleted: sha256:8fd6a1ece3ceceae6d714004614bae5b581c83ab962d838ef88ce760583dcb80
    Deleted: sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef
    ```

    c. Compruebe que la imagen se haya eliminado:

    ```
    docker images | grep hello-openshift
    ```

    Ejemplo de salida:

    ```
    REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
    ```

<a name="Build_Your_Own"> </a>

## Crea y ejecuta tu propia imagen

Usamos un podman `Containerfile` , que contiene las instrucciones para crear las nuevas capas de tu imagen. Para quienes están familiarizados con Docker, el `Containerfile` es equivalente a un `Dockerfile` .

> **Nota:** Además de un Containerfile, también puedes usar Dockerfiles existentes para crear una imagen con podman

Recuerde que una imagen contiene todo el sistema de archivos que desea utilizar para ejecutar su proceso virtual en un contenedor.

Para este ejemplo, estamos creando una nueva imagen para una aplicación web Java EE **ServletApp.war** . Está configurada para ejecutarse en WebSphere Liberty Runtime. El archivo de configuración del servidor se encuentra en **server.xml** .

1. Cambie el directorio a openshift-workshop-was/labs/Openshift/HelloContainer

    ```
    cd /home/techzone/appmod-pot-labfiles/labs/IntroContainers
    ```

2. Revise el `Containerfile` proporcionado desde el directorio:

    ```
    cat Containerfile
    ```

    Contenido del **archivo Containerfile** :

    ```
    FROM ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi
    COPY server.xml  /config
    COPY ServletApp.war /config/dropins/app.war
    RUN installUtility install --acceptLicense /config/server.xml
    ```

    - Para crear una nueva imagen, se comienza con una imagen preexistente. La primera línea `FROM` especifica la imagen existente que se utilizará como base. Si no se encuentra en el registro local, se obtendrá de un registro remoto, como Docker Hub. La imagen base que estamos utilizando, `ibmcom/websphere-liberty` , ya está empaquetada previamente y disponible en Docker Hub.

    - La segunda línea `COPY` es una copia directa del archivo `server.xml` desde el directorio local a `/config/server.xml` en la imagen. Esto agrega una nueva capa a la imagen con la configuración real del servidor que se utilizará.

    - La tercera línea, otra `COPY` , copia `ServletApp.war` del directorio actual a una nueva capa en la imagen que está creando, en la ubicación `/config/dropins/app.war` .

    - La última línea `RUN` ejecuta el comando `installUtility` dentro de la imagen para instalar las funciones adicionales necesarias para ejecutar el servidor como se especifica en `server.xml` . Puede usar el comando `RUN` para ejecutar cualquier comando que esté disponible dentro de la imagen para personalizar la imagen en sí.

     <br>
    

3. Ejecute la compilación. Asegúrese de incluir `.` al final del comando (el punto indica que se está utilizando el archivo del directorio actual):

    ```
    docker build -t app -f Containerfile .
    ```

    - La opción `-t` etiqueta el nombre de la imagen como `app` .
    - La opción `-f` especifica el nombre del `Containerfile` .
    - El comando de compilación ejecuta los comandos en `Containerfile` para crear una nueva imagen llamada `app` .

     <br>


    Ejemplo de salida:

    ```
    +] Building 37.3s (9/9) FINISHED
    => [internal] load build definition from Containerfile                                                                                              0.0s
    => => transferring dockerfile: 308B                                                                                                                 0.0s
    => [internal] load .dockerignore                                                                                                                    0.0s
    => => transferring context: 2B                                                                                                                      0.0s
    => [internal] load metadata for docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi                                                         0.1s
    => [1/4] FROM docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi@sha256:919c8624bc6259eafb4b76f2deada08574f933642c114d2bcbf6d7d488719ed0   0.0s
    => [internal] load build context                                                                                                                    0.0s
    => => transferring context: 66B                                                                                                                     0.0s
    => CACHED [2/4] COPY server.xml  /config                                                                                                            0.0s
    => CACHED [3/4] COPY ServletApp.war /config/dropins/app.war                                                                                         0.0s
    => [4/4] RUN installUtility install --acceptLicense /config/server.xml                                                                             36.9s
    => exporting to image                                                                                                                               0.2s
    => => exporting layers                                                                                                                              0.2s
    => => writing image sha256:cde953ee1a01310e425f334152be2463c0dce03b9d2fd337ec41a920e8ba5d7d                                                         0.0s
    => => naming to docker.io/library/app
    ```

4. Enumere las imágenes para ver que la nueva `app` de imágenes está creada:

    ```
    docker images | grep -B1 '\<app\>'
    ```

    Producción:

    ```
    REPOSITORY                            TAG                        IMAGE ID       CREATED         SIZE
    app                                   latest                     baa6bb9ad29d   2 minutes ago   544 MB
    ```

5. Inicie el contenedor.

    **Nota:** Está ejecutando puertos http y https:

    ```
    docker run -d -p 9080:9080 -p 9443:9443 --name=app-instance app
    ```

6. Acceda a la aplicación que se ejecuta en el contenedor:

    a. Abra el navegador web Firefox y vaya a la URL: `http://localhost:9080/app`

    b. Verifique que muestre una página que muestre `Simple Servlet ran successfully` .

    c. También apunte su navegador a 9443: `https://localhost:9443/app`

    > **Sugerencia:** Copie la URL incluyendo https, no cambie simplemente 9080 por 9443

    > Nota: Si recibe un banner de “riesgo de seguridad” debido al certificado autofirmado, simplemente acéptelo y continúe.

    ![Hola openshift](images/buildyourown1.png)

      <br>
    

7. Enumere los contenedores en ejecución:

    ```
    docker ps  | grep -B1 '\<app\>'
    ```

    Producción:

    ```
    CONTAINER ID     IMAGE     COMMAND                  CREATED             STATUS              PORTS                                NAMES
    595cdc49c710     app       "/opt/ibm/helpers/ru…"   8 minutes ago       Up 8 minutes        0.0.0.0:9080->9080/tcp, 0.0.0.0:9443->9443/tcp      app-instance
    ```

8. Acceda a los registros de su contenedor:

    ```
    docker logs -f app-instance
    ```

    Salida parcial: muestra la aplicación denominada "app" iniciada y el servidor de aplicaciones denominado "defaultServer" listo para ejecutarse.

    ```
     [AUDIT   ] CWWKZ0001I: Application app started in 0.346 seconds.
     [AUDIT   ] CWWKF0012I: The server installed the following features: [appSecurity-2.0, distributedMap-1.0, jndi-1.0, servlet-3.0, ssl-1.0].
     [AUDIT   ] CWWKF0011I: The defaultServer server is ready to run a smarter planet. The defaultServer server started in 3.106 seconds.
    ```

    a. Use `Ctrl-C` para salir de la visualización del registro

      <br>
    

9. Ingrese de forma remota a su contenedor en ejecución para realizar una exploración:

    ```
    docker exec -it app-instance /bin/sh
    ```

    En la sesión de shell,

    a. Ejecute `whoami` y luego ejecute `id` , tenga en cuenta que no está ejecutando como root.

    ```
     whoami

     id
    ```

    b. Tenga en cuenta que este es un entorno simplificado donde muchos comandos no están disponibles.

    Por ejemplo, pruebe los siguientes comandos:

    - **¿Qué ps** ver los procesos en ejecución? (tenga en cuenta que el comando **ps** no está disponible)
    - **cd /logs** para encontrar los archivos de registro.
    - **cd /liberty** para encontrar la ubicación de la instalación de liberty
    - **cd /liberty/usr/servers/defaultServer** para encontrar la configuración del servidor.
    - **cd /opt/ibm/wlp/output/defaultServer** para encontrar los archivos del área de trabajo requeridos por el entorno de ejecución del servidor

     <br>
    

10. Escriba `Exit` para salir del shell de Docker

    ```
    exit
    ```

    <br>

11. Limpieza:

    ```
    docker stop app-instance
    docker rm app-instance
    ```

12. Verifique que se haya eliminado la `app-instance` del contenedor Docker

    ```
    docker ps | grep app-instance

    docker ps -a | grep app-instance
    ```

    Ambos comandos no deberían devolver ningún resultado.

<a name="Versions"> </a>

## Administrar versiones de imágenes

No existe una versión integrada para las imágenes de contenedor. Sin embargo, puede utilizar una convención de etiquetado para crear versiones de sus imágenes. La convención es utilizar `major.minor.patch` , como `1.3.5` . La etiqueta predeterminada, si no especifica una, es `latest` .

Supongamos que la primera versión que crearemos para nuestro entorno es la 1.3.5. (Las versiones anteriores se crean en un entorno diferente).

1. Ejecute los comandos para etiquetar la última imagen `app` para nuestra primera versión:

    ```
    docker tag app app:1
    ```

    ```
    docker tag app app:1.3
    ```

    ```
    docker tag app app:1.3.5
    ```

2. Lista de imágenes de la aplicación:

    ```
    docker images | grep '\<app\>'
    ```

    Y el resultado:

    ```
    REPOSITORY                 TAG                        IMAGE ID            CREATED             SIZE
    app                        1                          d98cbdf82a0d        21 hours ago        542MB
    app                        1.3                        d98cbdf82a0d        21 hours ago        542MB
    app                        1.3.5                      d98cbdf82a0d        21 hours ago        542MB
    app                        latest                     d98cbdf82a0d        21 hours ago        542MB
    ```

    Tenga en cuenta que todas las diferentes etiquetas están actualmente asociadas con la misma imagen, ya que tienen el mismo ID de imagen.

    Después de etiquetar, el comando `docker run app:<version> ...` o `docker pull app:<version> ...` resolverá las versiones disponibles de la siguiente manera:

    - `app:1` se resuelve a la última versión 1.xx, que en este caso es `1.3.5` .
    - `app:1.3` se resuelve a la última versión 1.3.x, que en este caso es la `1.3.5`
    - `app:1.3.5` se resuelve a la versión exacta `1.3.5` .

    Después de crear una nueva imagen de parche que contenga correcciones de defectos, desea administrar las etiquetas de la nueva imagen para que un nuevo `docker run app:<version> ...` o `docker pull app:<version> ...` resuelva las imágenes de la siguiente manera:

    **Nota:** Probarás esto en los siguientes pasos:

    - `app:1.3.5` : se resuelve en la imagen `1.3.5` existente.
    - `app:1.3.6` : se resuelve en la nueva imagen
    - `app:1.3` : se resuelve en la nueva imagen.
    - `app:1` : se resuelve en la nueva imagen

     <br>
    

3. Simulemos la corrección de un defecto creando una nueva imagen usando `Containerfile1` en lugar de `Containerfile` :

    ```
    docker build -t app -f Containerfile1 .
    ```

    Ejemplo de salida parcial:

    ```
    [+] Building 0.5s (10/10) FINISHED
    => [internal] load .dockerignore                                                                   0.0s
    => => transferring context: 2B                                                                     0.0s
    => [internal] load build definition from Containerfile1                                            0.0s
    => => transferring dockerfile: 260B                                                                0.0s
    => [internal] load metadata for docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi        0.1s
    => [1/5] FROM docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi@sha256:919c8624bc6259ea  0.0s
    => [internal] load build context                                                                   0.0s
    => => transferring context: 66B                                                                    0.0s
    => CACHED [2/5] COPY server.xml  /config                                                           0.0s
    => CACHED [3/5] COPY ServletApp.war /config/dropins/app.war                                        0.0s
    => CACHED [4/5] RUN installUtility install --acceptLicense /config/server.xml                      0.0s
    => [5/5] RUN echo test1 > /config/test1                                                            0.3s
    => exporting to image                                                                              0.0s
    => => exporting layers                                                                             0.0s
    => => writing image sha256:6d4a772e1fd194e8786e3c2a89681da581d51fca884dad0a9633cf1d6a0e42e0        0.0s
    => => naming to docker.io/library/app
    ```

4. Etiqueta la imagen de la siguiente manera:

    ```
    docker tag app app:1
    docker tag app app:1.3
    docker tag app app:1.3.6
    ```

5. Verifique que estas tres imágenes tengan el mismo **ID DE IMAGEN,** lo que indica que son todas la misma imagen: `app:1` , `app:1.3` , `app:1.3.6` .

    ```
     docker images | grep '\<app\>'
    ```

    ```
    REPOSITORY                 TAG                        IMAGE ID       CREATED          SIZE
    app                        1                          6ae053c3a3de   42 seconds ago   537MB
    app                        1.3                        6ae053c3a3de   42 seconds ago   537MB
    app                        1.3.6                      6ae053c3a3de   42 seconds ago   537MB
    app                        latest                     6ae053c3a3de   42 seconds ago   537MB

    ```

    Una nueva versión secundaria implica cambios compatibles que van más allá de las correcciones de errores. Después de crear una nueva imagen de versión secundaria, conviene administrar las etiquetas de forma que:

    - `app:1.3.5` : se resuelve en la imagen `1.3.5` existente.
    - `app:1.3.6` : se resuelve en la imagen `1.3.6` existente
    - `app:1.4.0` : se resuelve en la nueva imagen
    - `app:1.3` : se resuelve a la imagen `1.3.6` existente.
    - `app:1.4` : se resuelve en la nueva imagen.
    - `app:1` : se resuelve en la nueva imagen

     <br>
    

6. Construya una nueva imagen usando `Containerfile2` :

    ```
    docker build -t app -f Containerfile2 .
    ```

    La salida parcial:

    ```
    [+] Building 0.4s (11/11) FINISHED
    => [internal] load .dockerignore                                                                   0.0s
    => => transferring context: 2B                                                                     0.0s
    => [internal] load build definition from Containerfile2                                            0.0s
    => => transferring dockerfile: 291B                                                                0.0s
    => [internal] load metadata for docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi        0.1s
    => [internal] load build context                                                                   0.0s
    => => transferring context: 66B                                                                    0.0s
    => [1/6] FROM docker.io/ibmcom/websphere-liberty:kernel-java8-ibmjava-ubi@sha256:919c8624bc6259ea  0.0s
    => CACHED [2/6] COPY server.xml  /config                                                           0.0s
    => CACHED [3/6] COPY ServletApp.war /config/dropins/app.war                                        0.0s
    => CACHED [4/6] RUN installUtility install --acceptLicense /config/server.xml                      0.0s
    => CACHED [5/6] RUN echo test1 > /config/test1                                                     0.0s
    => [6/6] RUN echo test2 > /config/test2                                                            0.2s
    => exporting to image                                                                              0.0s
    => => exporting layers                                                                             0.0s
    => => writing image sha256:e4b59ae187e74e06eba7bdd2bfef71eea586cbf31c569bf0590a2eeaae515997        0.0s
    => => naming to docker.io/library/app
    ```

7. Etiqueta la nueva imagen de la siguiente manera:

    ```
    docker tag app app:1
    docker tag app app:1.4
    docker tag app app:1.4.0
    ```

8. Utilice el comando `docker images | grep '\<app\>'` y verifique lo siguiente:

    - `1` , `1.4` y `1.4.0` son la misma imagen
    - `1.3` y `1.3.6` son la misma imagen

    ```
    REPOSITORY                 TAG                        IMAGE ID       CREATED          SIZE
    app                        1                          d37b46943cff   27 seconds ago   537MB
    app                        1.4                        d37b46943cff   27 seconds ago   537MB
    app                        1.4.0                      d37b46943cff   27 seconds ago   537MB
    app                        latest                     d37b46943cff   27 seconds ago   537MB
    app                        1.3                        6ae053c3a3de   4 minutes ago    537MB
    app                        1.3.6                      6ae053c3a3de   4 minutes ago    537MB

    ```

9. Eliminar las imágenes de Docker

    ```
    docker rmi app:1
    docker rmi app:1.4
    docker rmi app:1.4.0
    docker rmi app:1.3
    docker rmi app:1.3.5
    docker rmi app:1.3.6
    docker rmi app:latest
    ```

10. Verifique que se hayan eliminado las imágenes. El siguiente comando no debería tener ninguna imagen en la lista.

```
docker images | grep '\<app\>'
```

<br>

¡Felicitaciones! Has completado el laboratorio **Introducción a los contenedores** .

**===== FIN DEL LABORATORIO =====**

[Regresar a la página principal de laboratorios](../README.md)
