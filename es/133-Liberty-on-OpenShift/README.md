# Implemente y configure rápidamente sus aplicaciones Java con WebSphere Liberty y OpenShift Container Platform

![bandera](./images/media/image1.jpeg)

**Última actualización:** marzo de 2024

**Duración:** 90 minutos

¿Necesita ayuda? Póngase en contacto con **Kevin Postreich, Yi Tang**

[Regresar a la página principal de laboratorios](../README.md)

## Descripción general

Este laboratorio proporciona experiencia práctica fundamental en la modernización de aplicaciones Java existentes a WebSphere Liberty, implementadas en una plataforma de contenedores, como Red Hat OpenShift.

Este laboratorio se centra en los aspectos prácticos de la implementación y configuración de aplicaciones, y no en el análisis de los resultados de Transformation Advisor. *Otros laboratorios cubren Transformation Advisor en detalle.*

Al finalizar este laboratorio, habrá adquirido habilidades para descargar y usar el `Transformation Advisor migration bundle` para implementar y configurar su aplicación en los siguientes escenarios:

1. **a un WebSphere Liberty que se ejecuta localmente** : útil para implementar en Liberty en una máquina virtual

2. **a una imagen de contenedor** : útil para implementar en Kubernetes

3. **Uso del operador Liberty** : útil para implementar en OpenShift

**IBM Cloud Transformation Advisor** (Transformation Advisor) es una herramienta de modernización de aplicaciones que se comercializa a través de IBM Cloud Pak for Applications y WebSphere Hybrid Edition. Transformation Advisor le ayuda a evaluar rápidamente las aplicaciones Java EE locales para implementarlas en la nube.

La herramienta Transformation Advisor proporciona el siguiente valor:

- Identifica los modelos de programación Java EE en la aplicación.

- Determina la complejidad de la reestructuración de estas aplicaciones al enumerar un inventario de alto nivel del contenido y la estructura de cada aplicación.

- Aspectos destacados del modelo de programación Java EE y las diferencias de la API de WebSphere entre los tipos de perfil de tiempo de ejecución de WebSphere

- Identifica las diferencias de implementación de la especificación Java EE que podrían afectar la aplicación.

- Genera aceleradores para implementar la aplicación en Liberty y contenedores en un entorno de destino

Además, la herramienta ofrece recomendaciones sobre la edición de IBM WebSphere Application Server más adecuada y ofrece consejos, mejores prácticas y posibles soluciones para evaluar la facilidad de trasladar aplicaciones a Liberty o a versiones más nuevas de WebSphere tradicional. Acelera el proceso de migración de aplicaciones a la nube, minimiza los errores y los riesgos y reduce el tiempo de comercialización.

## Objetivo

Los objetivos de este laboratorio son:

- Aprenda a descargar el paquete de migración de aplicaciones y a usarlo para implementar una aplicación en WebSphere Liberty ejecutándose localmente

- Aprenda a rellenar por completo los marcadores de posición del paquete de migración y a crear la aplicación en una imagen de contenedor.

- Conozca el rol de Kustomize al implementar el paquete de migración

- Aprenda a implementar su aplicación en OpenShift con un solo comando

- Aprenda a crear múltiples configuraciones para la aplicación e implementarlas en OpenShift

## Prerrequisitos

Se deben completar los siguientes requisitos previos antes de comenzar este laboratorio:

- Familiaridad con los comandos básicos de Linux

- Tener acceso a Internet

- Acceso al entorno de laboratorio TechZone

## Accediendo al entorno del laboratorio

Si está realizando este laboratorio como parte de un taller dirigido por un instructor (virtual o presencial), ya se le ha proporcionado un entorno. El instructor le proporcionará los detalles para acceder al entorno del laboratorio.

De lo contrario, deberá reservar un entorno para el laboratorio. Puede obtener uno aquí. Siga las instrucciones en pantalla para la opción “ **Reservar ahora** ”.

[https://techzone.ibm.com/my/reservations/create/65eb652a747d7a00108fb5db](https://techzone.ibm.com/my/reservations/create/65eb652a747d7a00108fb5db)

El entorno de laboratorio contiene seis (6) máquinas virtuales Linux.

![Captura de pantalla de un error informático Descripción generada automáticamente](./images/media/image2.png)

1. Acceda al entorno de laboratorio desde su navegador web.

    Se configura un `Published Service` para proporcionar acceso a la máquina virtual **`Workstation`** a través de la interfaz noVNC para el entorno de laboratorio.

    a. Cuando se haya aprovisionado el entorno de demostración, haga clic en el **`environment tile`** para abrir su vista de detalles.

    b. Haga clic en el enlace **`Published Service`** que mostrará una **`Directory listing`**

    c. Haga clic en el enlace **`vnc.html`** para abrir el entorno de laboratorio a través de la interfaz **`noVNC`** .

    ![Una lista de descripción de listados de directorios generada automáticamente](./images/media/image3.png)

    d. Haga clic en el botón **`Connect`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image4.png)

    e. Ingrese la contraseña como: **`passw0rd`** . Luego haga clic en el botón **`Send Credentials`** para acceder al entorno de laboratorio.

    Nota: Ese es un cero numérico en passw0rd

    ![Una captura de pantalla de un cuadro de inicio de sesión Descripción generada automáticamente](./images/media/image5.png)

2. Si se le solicita iniciar sesión en la máquina virtual de la "estación de trabajo", utilice las credenciales a continuación:

> Las credenciales de inicio de sesión para la **estación de trabajo** VM son:

- ID de usuario: **techzone**

- Contraseña: **IBMDem0s!**

> Nota: Ese es un cero numérico en la contraseña.
>
> ![pantalla de vm de estudiante](./images/media/image6.png)

## Consejos para trabajar en el entorno de laboratorio

1. Puede cambiar el tamaño del área visible utilizando las opciones **de Configuración de noVNC** para cambiar el tamaño del escritorio virtual para que se ajuste a su pantalla.

    a. Desde la máquina virtual del entorno, haga clic en el **`twisty`** en el panel de control noNC para abrir el menú.

    ![ajustar a la ventana](./images/media/image7.png)

    b. Para aumentar el área visible, haga clic en **`Settings > Scaling Mode`** y configure el valor en **`Remote Resizing`**

    ![ajustar a la ventana](./images/media/image8.png)

2. Puede copiar/pegar texto de la guía de laboratorio en el entorno de laboratorio utilizando el portapapeles en el visor noVNC.

    a. Copie el texto de la guía de laboratorio que desea pegar en el entorno de laboratorio.

    b. Haga clic en el icono **`Clipboard`** y **`paste`** el texto en el portapapeles de noVNC

    ![ajustar a la ventana](./images/media/image9.png)

    c. Pegue el texto en la máquina virtual, por ejemplo en una ventana de terminal, una ventana del navegador, etc.

    d. Haga clic en el icono **`clipboard`** nuevamente para cerrarlo.

    Como alternativa a la opción Copiar/Pegar de noVNC, puede considerar abrir la guía de laboratorio en un navegador web dentro de la máquina virtual. Con este método, puede copiar y pegar fácilmente texto de la guía de laboratorio sin tener que usar el portapapeles de noVNC.

## Tareas de laboratorio

En este laboratorio, utilizará el `migration bundle` de Transformation Advisor para crear una imagen de contenedor e implementar una aplicación en un contenedor local para realizar pruebas.

Luego, implementará la aplicación en entornos de “desarrollo” y “preparación” en OpenShift.

Aprovechará `WebSphere Liberty Operator` , Kustomize (un marco de gestión de configuración nativo de Kubernetes) y los artefactos de implementación generados por el paquete de migración de IBM Cloud Transformation Advisor.

Para simplificar el laboratorio y permitirle centrarse en el paquete de migración, ya se han implementado determinados programas y artefactos. Son los siguientes:

- Se ha instalado **Transformation Advisor** y se han cargado los datos recopilados

- Se ha instalado **Docker** (para crear y ejecutar imágenes)

- Se ha instalado **oc** (herramienta de línea de comandos OpenShift para ejecutar comandos OCP)

- La **aplicación de muestra PlantsByWebSphere** , creada como un archivo Enterprise Archive (EAR), está disponible en el recurso de laboratorio proporcionado.

- Se utilizarán **scripts de conveniencia** para agilizar tareas, ahorrándole tiempo y permitiéndole centrarse en el valor y los resultados, que idealmente también estarían programados en su entorno.

# Parte 1: Evaluación de la solicitud

## 1.1 Despliegue los artefactos del laboratorio y configure el entorno del laboratorio

1. Si aún no ha clonado el repositorio de GitHub con los artefactos del laboratorio, en un laboratorio anterior, ejecute el siguiente comando en su terminal:

    a. Abra una nueva ventana de Terminal y clone el repositorio git para descargar los artefactos del laboratorio.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image10.png)

    b. Ejecute los comandos para clonar el repositorio git en el sistema local

    ```
    cd /home/techzone

    git clone https://github.com/IBMTechSales/appmod-pot-labfiles.git
    ```

    c. **Agregar permisos de “ejecución” a los scripts de shell**

    ```
    find ./appmod-pot-labfiles -name "*.sh" -exec chmod +x {} \;
    ```

2. Ejecute el script de shell proporcionado para configurar el entorno de laboratorio.

    El script lab-setup.sh mueve archivos del repositorio git clonado a un directorio **de estudiantes** utilizado en el laboratorio.

    ```
    cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

    ./lab-setup.sh
    ```

    Cuando termine, verá el siguiente resultado

    ==========================

    Se completó el script de configuración del laboratorio

    ==========================

## 1.2 Asesor de Transformación de Lanzamiento (local)

Transformation Advisor ofrece un **`migration plan`** para cada aplicación que se evalúa para su modernización. El plan de migración incluye un **`migration bundle`** de artefactos generados que aceleran la implementación de aplicaciones en Liberty en contenedores y Kubernetes/OpenShift.

El paquete de migración incluye diversos artefactos, según las necesidades de la aplicación para acelerar la creación y la implementación de una imagen de contenedor de la aplicación en una plataforma de contenedores OpenShift.

El Asesor de Transformación se instala localmente en la máquina virtual **de la estación de trabajo** .

1. Inicie la herramienta Transformation Advisor siguiendo los pasos a continuación.

    a. Desde la barra de herramientas del escritorio de **Workstation** VM, haga clic en el ícono `Terminal` para abrir una ventana de Terminal.

    ![](./images/media/image11.png)

    b. Inicie el **Asesor de transformación** con los comandos:

    ```
     cd /home/techzone/transformation-advisor-local-3.8.1

     ./launchTransformationAdvisor.sh
    ```

    Espere a que Transformation Advisor se inicialice y muestre la lista **del menú de acciones** .

    c. Escriba **`5`** y presione **`Enter`** para “Iniciar” el **Asesor de Transformación** .

    ![](./images/media/image12.png)

    d. Se inicia la aplicación **Transformation Advisor** , **`right-click`** en el enlace URL de la aplicación y seleccione **`Open Link`** para iniciarla en una ventana del navegador web.

    La URL se muestra en la salida del comando TA: [**http://server0.gym.lan:3000**](http://server0.gym.lan:3000)

    ![](./images/media/image13.png)

    La página de inicio **de Transformation Advisor** se muestra en el navegador web.

    ![](./images/media/image14.png)

    En la siguiente sección, creará un nuevo “ **Espacio de trabajo** ” en Transformation Advisor y cargará los resultados guardados del escaneo de un WebSphere Application Server que tiene una sola aplicación implementada, denominada “PlantsByWebSphere”.

## 1.3 Crear un nuevo espacio de trabajo y cargar los resultados del análisis desde un servidor de aplicaciones WebSphere

En esta sección, cree un nuevo espacio de trabajo llamado **'proof_of_concept'** .

Un `workspace` es un área designada que albergará las recomendaciones de migración proporcionadas por Transformation Advisor en función de los resultados del análisis de Data Collector de su entorno de servidor de aplicaciones.

1. Crear un nuevo espacio de trabajo llamado **prueba de concepto**

    a. Desde la página de inicio de Transformation Advisor, haga clic en el botón **`Create New`**

    b. Escriba **`proof_of_concept`** como nombre del espacio de trabajo. Luego, haga clic en el botón **`Create`** .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image15.png)

2. Cargue un archivo de **'resultados de escaneo'** existente proporcionado para el laboratorio.

    Se le proporciona el archivo con los resultados del escaneo.

    Se produjo ejecutando el **recopilador de datos** de Transformation Advisor en un servidor de aplicaciones WebSphere que tenía implementada la aplicación **PlantsByWebSphere** .

    a. Haga clic en el botón **`Upload`**

    b. Haga clic en el enlace **`Drop or add file`** . Luego, haga clic en el botón **`Upload`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image16.png)

    c. Vaya a **Inicio &gt; Techzone &gt; appmod-pot-labfiles &gt; labs &gt; Modernización del tiempo de ejecución**

    d. Seleccione el archivo **`pbw-collection.zip`** . Luego haga clic en el botón **`Open`** .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image17.png)

    e. Una vez agregado el **archivo pbw-collection.zip** , haga clic en el botón **`Upload`** para cargar los resultados en Transformation Advisor.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image18.png)

    Después de unos momentos, los datos de la aplicación se cargarán en la interfaz de usuario de Transformation Advisor.

    El **espacio de trabajo de prueba de concepto** contiene datos de una única aplicación que se utiliza como caso de prueba. Ahora revisaremos esta aplicación en Transformation Advisor.

    El espacio de trabajo " **proof_of_concept** " muestra la página " **Todas las aplicaciones Java** ", que muestra las recomendaciones para el espacio de trabajo.

    Hay una única aplicación llamada **plantsbywebsphereee6.ear** .

    De forma predeterminada, se selecciona "WebSphere Liberty" como destino de modernización. Toda la información proporcionada supone que esta aplicación se modernizará a WebSphere Liberty.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image19.png)

## 1.4 Analizar la aplicación plantsbywebsphereee6

Transformation Advisor proporciona información detallada sobre cada aplicación que se ha analizado. Esta aplicación tiene una complejidad de **`Simple`** .

**Simple** significa:

- La aplicación está lista para implementarse en WebSphere Liberty
- No se requieren cambios en el código fuente.

---

**Nota:** Describiremos parte de la otra información que se muestra en Transformation Advisor más adelante en el laboratorio.

---

![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image20.png)

***¿Cómo está lista esta aplicación para su implementación cuando muestra 4 problemas?***

En este caso la aplicación tiene cuatro problemas **`Informational`** .

Los problemas de información no impiden que la aplicación se ejecute en el nuevo entorno de ejecución (WebSphere Liberty), pero puede haber pequeños cambios en el comportamiento de la aplicación.

Si se encuentra un comportamiento inesperado durante la prueba, revisar estos problemas **informativos** puede ayudar a explicar lo que está sucediendo.

1. Ahora estamos listos para revisar el **'plan de migración'** para esta aplicación.

    a. Haga clic en el enlace **`Migration plan`** al final de la fila de la aplicación **plantsbywebsphereee6.ear** .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image21.png)

    b. Se abre la página **Plan de migración** .

    Esta página contiene un resumen de la aplicación que se está migrando:

    - una vista previa de los archivos que ayudarán durante la implementación (resaltados a continuación)

    - una lista de las dependencias de la aplicación.

    Todos los archivos se pueden descargar en un único y práctico **paquete de migración** .

    ![](./images/media/image22.png)

2. En la parte inferior de la pantalla, hay una sección `Appliction Dependencies` .

    Aquí se muestran todos los archivos, además de la aplicación, que se requieren para una implementación. **Plantsbywebspheeee6.ear** tiene dos dependencias.

    a. Expande la sección para ver los detalles.

    b. En este caso, se requieren los controladores DB2 denominados **db2jcc.jar** y **db2cc_licence.jar** para la implementación.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image23.png)

3. Descargue el paquete de migración haciendo clic en el botón **`Download`** en la parte inferior derecha.

    Se descargará un archivo zip llamado “ **plantsbywebsphereee6.ear_migrationBundle.zip** ” en su carpeta **de descargas** .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image24.png)

Realizaremos un proceso paso a paso sobre cómo utilizar el paquete de migración de Transformation Advisor para implementar **plantsbywebsphereee6.ear** de la siguiente manera:

- Asegúrese de que la aplicación pueda ejecutarse localmente en WebSphere Liberty

- Cree una imagen de contenedor inmutable que se ejecute en WebSphere Liberty

- Asegúrese de que la aplicación pueda ejecutarse localmente en WebSphere Liberty en un contenedor

- Implemente la imagen en el entorno “ **dev** ” de OpenShift (y configúrelo) usando un solo comando

- Vuelva a implementar y configurar la imagen para un entorno de “ **preparación** ” con un solo comando

# Parte 2: Modernización del entorno de ejecución

## 2.1 Modernización del entorno de ejecución

**La modernización del entorno de ejecución** traslada una aplicación a un entorno de ejecución "creado para la nube" con el mínimo esfuerzo. WebSphere Liberty es un servidor de aplicaciones Java rápido, dinámico y fácil de usar.

Ideal para la nube y los contenedores, Liberty es de código abierto, con tiempos de inicio rápidos, sin reinicios del servidor para detectar cambios y una configuración XML simple.

Las aplicaciones implementadas en el entorno de ejecución del contenedor WebSphere Liberty se pueden crear, implementar y administrar con las mismas tecnologías y metodologías comunes que utilizarían las aplicaciones nativas de la nube (creadas para la nube).

### 2.1.1 Implementar la aplicación PlantsByWebSphere en un WebSphere Liberty que se ejecuta localmente

En este entorno de laboratorio, WebSphere Liberty se instala localmente en la máquina virtual de la estación de trabajo.

En este paso, creará un nuevo servidor Liberty para ejecutar la aplicación PlantsByWebSphere.

Luego, revisará los `placeholder files` en el paquete de Transformation Advisor para ver los archivos de dependencia que deben copiarse en Liberty Server.

Luego, utilizará el archivo `server.xml` generado por Transformation Advisor, e incluido en el paquete de migración, para configurar el servidor Liberty para la aplicación.

### 2.1.2 Crear un servidor Liberty local

1. Desde una ventana **`Terminal`** , cree un nuevo servidor WebSphere Liberty local llamado " **pbwserver** "

    ```
    /home/techzone/wlp/bin/server create pbwserver
    ```

    **producción:**

    ```
    Server pbwserver created.
    ```

2. Desde la ventana **Terminal** , inicie el servidor WebSphere Liberty local.

    ```
    /home/techzone/wlp/bin/server start pbwserver
    ```

    **Producción:**

    ```
    Starting server pbwserver.

    Server pbwserver started with process ID #####
    ```

3. Confirme que el servidor WebSphere Liberty local esté en ejecución. Abra el navegador web `Firefox` y abra una nueva pestaña del navegador. Luego, vaya a: **http://localhost:9080**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image25.png)

4. Actualmente, la aplicación PlantsByWebSphere NO está implementada en el servidor Liberty. Además, el servidor Liberty NO está configurado para ejecutar la aplicación.

    a. Desde el navegador web, intente acceder a la aplicación:

    [**http://localhost:9080/PlantasPorWebSphere**](http://localhost:9080/PlantsByWebSphere)

    Recibirá el mensaje “ **No se encontró la raíz del contexto** ”. Esto es normal, ya que la aplicación aún no se ha implementado en el servidor Liberty.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image26.png)


<br>


### 2.1.3 Revisar los archivos de marcador de posición en el paquete de migración

En esta sección, revisará el paquete de migración para ver qué archivos deben agregarse a la configuración de WebSphere Liberty para ejecutar la aplicación.

Luego, utilizará el archivo de configuración de Liberty proporcionado, **`server.xml`** , para configurar el servidor Liberty.

El archivo `server.xml` se utiliza para configurar Liberty proporcionando valores para puertos, seguridad y rutas de contexto, y proporcionando configuración específica de la aplicación, como acceso a fuentes de datos de la aplicación.

1. Tomemos nota de los 3 `placeholder files` que se incluyen en el `migration bundle` .

    Los archivos de marcador de posición son referencias convenientes que le permitirán saber qué archivos necesitará copiar al **servidor Liberty** .

    *Nota: Completarás completamente el paquete de migración en pasos posteriores* .

    a. Desde la **ventana Terminal** , enumere los archivos de marcador de posición en el paquete de migración.

    ```
    unzip -l /home/techzone/Downloads/plantsbywebsphereee6.ear\_migrationBundle.zip | grep placeholder
    ```

    ![](./images/media/placeholder-files.png)

    - El directorio **`target`** contiene el archivo de marcador de posición para el archivo de implementación EAR de la aplicación PlantsByWebSphere. Este es un recordatorio de que debe copiar el archivo EAR de PlantsByWebSphere en el servidor Liberty.

    - El directorio **`src/main/liberty/lib`** contiene los archivos de marcador de posición para las bibliotecas de bases de datos DB2 que requiere la aplicación. Este es un recordatorio de que debe copiar las bibliotecas DB2 al servidor Liberty.

    <br>

### 2.1.4 Configurar el servidor Liberty para PlantsByWebSphere

En esta sección, copiará los archivos de dependencia necesarios y la configuración del servidor al servidor Liberty.

- Copie las **bibliotecas DB2** al directorio definido en la "Configuración del controlador DB2" en el archivo server.xml.

- Copie el archivo **EAR de PlantsByWebSphere** al directorio “apps” del servidor Liberty

- Copie el archivo **server.xml** del paquete de migración al servidor Liberty, reemplazando el archivo server.xml predeterminado

Para facilitar la ejecución de los pasos `copy` anteriores, hemos proporcionado un `convenience shell script` que crea los directorios necesarios y copia los archivos al servidor Liberty.

Ejecutará el script, que le solicitará que presione la tecla “ **Enter** ” antes de ejecutar cada uno de los comandos del script. Esto le permite examinar el comando, evitando tener que realizar actividades rutinarias de copiar y pegar en el laboratorio.

<br>

**El script realiza los siguientes pasos por usted.**

- **Paso 1:** Crea un nuevo directorio donde se extraerá el paquete de migración

- **Paso 2** : Descomprime el paquete de migración, reemplazando el que habías extraído previamente en el laboratorio.

- **Paso 3:** crear el directorio " **lib/global** " en el servidor Liberty, donde se copiarán las bibliotecas de DB2. Este directorio coincide con la ubicación definida en el **archivo server.xml**

- **Paso 4:** Copie las bibliotecas del controlador DB2 al directorio "lib/global" del servidor Liberty

- **Paso 5:** Copiar el archivo EAR de la aplicación PlantsByWebSphere al servidor Liberty. El archivo EAR se copia al directorio “ **apps** ” del servidor.

- **Paso 6:** Copie el archivo **server.xml** del paquete de migración al servidor Liberty. Esto reemplaza el archivo server.xml predeterminado existente que se creó cuando creó el servidor.

<br>

1. Ejecute el script de shell `local-liberty-config.sh` para configurar el servidor Liberty como se indicó anteriormente.

    ```
    cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

    ./local-liberty-config.sh -i
    ```

    Examine el comando que se va a ejecutar. Luego, presione la tecla `Enter` cuando esté listo para que el script ejecute el comando por usted.

    Nota: El parámetro “ `-i` ” le indica al script que se ejecute en modo interactivo y le solicita que presione la **tecla `Enter`** para ejecutar el comando mostrado.

    <br>

      <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Para referencia: Las capturas de pantalla de cada paso del script "local-liberty-config"</summary>

    <br>

    ![Captura de pantalla de un mensaje de error de computadora Descripción generada automáticamente](./images/media/image29.png)

    ![Una pantalla blanca con texto negro Descripción generada automáticamente](./images/media/image30.png)

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image31.png)

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image32.png)

    ![Captura de pantalla de computadora de un código de computadora Descripción generada automáticamente](./images/media/image33.png)

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image34.png)




    ---

    El siguiente mensaje se muestra cuando se completan todos los pasos del script.

    ![Un primer plano de una descripción de código generada automáticamente con confianza media](./images/media/image35.png)

    ---

2. Desde una ventana **de Terminal** , `start` el servidor Liberty llamado " **pbwserver** "

    ```
    /home/techzone/wlp/bin/server start pbwserver
    ```

3. Vuelva a ejecutar la aplicación PlantsByWebSphere desde el navegador.

    ```
    http://localhost:9080/PlantsByWebSphere
    ```

    Ahora la aplicación se está ejecutando en Liberty y se muestra la página principal.

    ![Página web de un jardín Descripción generada automáticamente](./images/media/image36.png)

4. Intente hacer clic en cualquiera de las pestañas: “ **Flores** ”, **“Frutas y verduras** ” o “ **Árboles** ”.

    ---

    **Tenga en cuenta la excepción.** Se trata de un error esperado. El problema está en la persistencia de JPA (acceso a la base de datos). No se ha definido un usuario y, por lo tanto, la autenticación a la base de datos ha fallado.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image37.png)

    ---

    Estas páginas deben mostrar un catálogo de elementos en su respectiva categoría, que se obtienen de la base de datos de la aplicación.

    **¿Y entonces qué pasó?**

    Transformation Advisor no recopila ningún `sensitive data` del servidor de aplicaciones. Esto significa que no se ha establecido la información de configuración específica de la aplicación en el archivo **server.xml** . En este caso, faltan el `username` y `password` para acceder a la base de datos.

    En la siguiente sección, revisará el archivo server.xml y agregará los datos confidenciales necesarios para acceder a la base de datos de la aplicación.

    <br>

### 2.1.5 Revisar el archivo server.xml

Ahora revisará el archivo **`server.xml`** y establecerá la información de configuración necesaria.

El archivo **server.xml** define un conjunto de **`features`** que requiere la aplicación. Al importar solo las **características** necesarias para satisfacer las necesidades de API de la aplicación, el espacio ocupado por la aplicación implementada y el servidor Liberty se mantiene lo más pequeño posible.

1. Revisar el archivo server.xml

    Usando el editor “ **`gedit`** ” en una ventana **`Terminal`** , abra el archivo **server.xml** ubicado en el servidor Liberty

    ```
     gedit /home/techzone/wlp/usr/servers/pbwserver/server.xml
    ```

    - **La sección 1** contiene las `features` que requiere la aplicación, y estas se descubren automáticamente durante el análisis que realiza el Transformation Advisor **Data Collector** .

        ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image38.png)

    - **Sección 2:** Define los `resources` necesarios para acceder a la base de datos

        - Los **`authdata`** desafían el usuario y la contraseña de BD2 que utilizan las fuentes de datos. Estos hacen referencia a variables que se definen en el archivo server.xml.

        - El controlador **`jdbc`** define las bibliotecas necesarias. Estas son las bibliotecas que copió en esta ubicación mediante el script

        ![Un código de computadora con texto verde y blanco Descripción generada automáticamente](./images/media/image39.png)

    - **Sección 3:** La **`datasource`** contiene toda la información necesaria para acceder a la base de datos: usuario de la base de datos, contraseña, nombre de la base de datos, host y número de puerto.

        ![Una captura de pantalla de computadora de un programa Descripción generada automáticamente](./images/media/image40.png)

    - **La sección 4** contiene las variables para los **`non-sensitive configuration data`** . Por ejemplo, el puerto en el que se ejecutará el servidor. Transformation Advisor recopila los valores de estas variables.

        ![Una captura de pantalla de un código de computadora Descripción generada automáticamente](./images/media/image41.png)

    - **La sección 5** contiene las **`sensitive data variables`** .

        Puede ver que los valores de estas variables están todos **en blanco** , ya que Transformation Advisor **nunca** recopila esta información.

        ![Una captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image42.png)

2. La razón por la que la aplicación **PlantsByWebSphere** devolvió una “Excepción” es porque no se han establecido los valores de las **variables sensibles** .

    ![](./images/media/image43.png)

    A continuación, actualizará el server.xml para incluir las credenciales necesarias para acceder a la base de datos de la aplicación.

### 2.1.6 Actualice el server.xml y vuelva a probar la aplicación PlantsByWebSphere

1. Desde una ventana de Terminal, `stop` el servidor Liberty llamado “pbwserver”

    ```
     /home/techzone/wlp/bin/server stop pbwserver
    ```

2. Desde el editor `gedit` que tiene abierto, actualice los valores en el archivo **server.xml** para los datos confidenciales como se ilustra a continuación:

    a. Desplácese hasta la parte inferior del archivo server.xml para ver las variables de datos confidenciales.

    b. Establezca el valor predeterminado para **rhel9_baseNode01_pbwuser_password** en: **`db2inst1-pwd`**

    c. Establezca el valor predeterminado para **rhel9_baseNode01_pbwuser_user** en: **`db2inst1`**

    ![](./images/media/image44.png)

    d. **`Save`** y **`close`** el archivo server.xml en el editor.

3. Desde una ventana de Terminal, `start` el servidor Liberty llamado “pbwserver”

    ```
     /home/techzone/wlp/bin/server start pbwserver
    ```

4. Recargue y pruebe la aplicación PlantsByWebSphere en el navegador

    ```
     http://localhost:9080/PlantsByWebSphere
    ```

    a. Haga clic en la pestaña “ **Flores** ”. Ahora debería aparecer el catálogo de flores.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image45.png)

5. Desde una ventana **de Terminal** , `Stop` el servidor Liberty llamado " **pbwserver** "

    ```
     /home/techzone/wlp/bin/server stop pbwserver
    ```

### Control

En este punto del laboratorio, ha demostrado con éxito la "Modernización en tiempo de ejecución" de la aplicación PlantsByWebSphere a WebSPhere Lberty en una máquina virtual local.

En las siguientes secciones, realizará una "Modernización operativa" mediante la cual implementará la aplicación PlantsByWebSphere en contenedores y Red Hat OpenShift.

# Parte 3: Modernización operativa

## 3.1 Modernización operativa

La modernización operativa brinda al equipo de operaciones la oportunidad de adoptar las mejores prácticas operativas modernas sin imponer requisitos de cambio al equipo de desarrollo.

Las funcionalidades de escalabilidad, enrutamiento, agrupamiento, alta disponibilidad y disponibilidad continua que antes proporcionaba el middleware del servidor de aplicaciones ahora pueden ser proporcionadas por la plataforma de contenedores.

Esto permite al equipo de operaciones ejecutar aplicaciones nativas de la nube y modernizadas en el mismo entorno con los mismos marcos estandarizados de registro, monitoreo y seguridad.

## 3.1.1 Explorar el Containerfile utilizado para crear PlantsByWebSphere como una imagen de contenedor

En la sección anterior, utilizó el **archivo server.xml** para ejecutar la aplicación en una instancia local de Liberty. Esto fue para mostrarle cómo se utiliza el archivo **server.xml** de Transformation Advisor. Si está migrando a Liberty en máquinas virtuales como su nuevo entorno de ejecución, ¡ya está listo!

Sin embargo, si los contenedores serán su destino final, esta sección explora los artefactos del paquete de migración de Transformation Advisor que aceleran la implementación de aplicaciones en Lberty en contenedores.

En el caso de las aplicaciones **`Simple`** , no es necesario realizar un paso independiente de implementación en una instancia local de Liberty. En cambio, al usar el paquete de migración de Transformation Advisor, puede implementar su aplicación en Liberty ejecutándose en un contenedor de una sola vez. Esto es lo que haremos ahora.

---

Para esta sección del laboratorio, el paquete de migración ya se ha actualizado de la siguiente manera:

- El archivo **EAR de PlantsByWebSphere** se ha añadido al paquete de migración y se ha eliminado su marcador de posición.

- Las **bibliotecas DB2** se han agregado al paquete de migración y se han eliminado los marcadores de posición.

- El archivo **server.xml** está configurado con valores predeterminados para el acceso a la base de datos.

---

1. El `migration bundle` está listo para usarse para generar una imagen de su aplicación ejecutándose en WebSphere Liberty. Para ello, utilizaremos el **`Containerfile`** que viene con el paquete de migración.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image49.png)

    a. Explora el Containerfile en el paquete de migración

    ```
    gedit /home/techzone/Student/labs/appmod/pbw-bundle-complete/Containerfile
    ```

    - Las instrucciones **FROM** en el **Containerfile** extraen las siguientes dos imágenes.

        - **`Open JDK 8`**
        - Versión **23.0.0.12** de **`WebSphere Liberty`** .

        ![](./images/media/image50.png)

        ![](./images/media/image51.png)

        ---

        **Nota:** El archivo Containerfile se modificó para este laboratorio para extraer `23.0.0.12` de `WebSphere Liberty` . De manera predeterminada, extrae la versión más reciente de WebSphere Liberty.

        ---

    - Los comandos **RUN** en el Containerfile crean las estructuras de carpetas necesarias y copian los **`binary files`** del paquete de migración en las ubicaciones adecuadas en la imagen.

        ![Un primer plano de un código de computadora Descripción generada automáticamente](./images/media/image52.png)

    - Hay varias líneas en el archivo que han sido comentadas.

        De forma predeterminada, **Containerfile** asume que su aplicación está disponible como archivo **binario** . Sin embargo, también se puede utilizar para crear su aplicación a partir del código **fuente** . Los detalles completos sobre cómo hacerlo se pueden encontrar en **README.md** en el paquete de migración.

        ![](./images/media/image53.png)

    b. `Close` el editor "gedit". **¡NO GUARDE NINGÚN CAMBIO EN EL ARCHIVO!**

    <br>

### 3.1.2 Configurar, crear y ejecutar PlantsByWebSphere en un contenedor local

Para que la aplicación PlantsByWebSphere se ejecute correctamente en un contenedor local, requiere acceso a la base de datos de la aplicación, que se ejecuta en un contenedor separado.

El contenedor de la aplicación y el contenedor de la base de datos deben estar conectados a la misma red Docker local.

En este laboratorio, los pasos para crear, configurar y ejecutar la aplicación PlantsByWebSphere en un contenedor local son:

- **Paso 1:** Asegúrese de que la base de datos de la aplicación DB2 esté en ejecución

- **Paso 2:** Crear una red Docker local

- **Paso 3:** Asegúrese de que se haya creado la red Docker

- **Paso 4:** Conecte el contenedor de base de datos DB2 a la red Docker

- **Paso 5:** Cree y etiquete la imagen del contenedor de la aplicación

- **Paso 6:** Iniciar la aplicación en el contenedor

Para facilitar la ejecución de los pasos anteriores, hemos proporcionado un `convenience shell script` que realiza estos pasos.

**Ejecutará el script** , que le solicitará que presione la tecla “ **Enter** ” antes de ejecutar cada uno de los comandos del script. Esto le permite examinar el comando antes de que se ejecute, evitando al mismo tiempo tener que realizar actividades rutinarias de copiar y pegar en el laboratorio.

1. Ejecute el siguiente comando para configurar, crear y ejecutar la aplicación PlantsByWebSphere en un contenedor local.

    ```
     cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

     ./build-container -i
    ```

    **Nota:** El parámetro `-i` le indica al script que se ejecute en modo interactivo y le solicita que presione la tecla **`Enter`** para ejecutar el comando mostrado.

    ---

    **Nota:** El script también realiza cualquier tarea de limpieza necesaria, en caso de que necesite volver a ejecutar el script por alguna razón desconocida.

    ---

        <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Para referencia: Las capturas de pantalla de cada paso del script "build-container"</summary>

    ![Un libro blanco con texto negro Descripción generada automáticamente](./images/media/image54.png)

    ![Captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image55.png)

    ![Captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image56.png)

    ![Una captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image57.png)

    ![Una captura de pantalla de un código de computadora Descripción generada automáticamente](./images/media/image58.png)

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image59.png)

    **Nota:** El comando `docker run` contiene algunos parámetros que se definen a continuación:

    -d: inicia el contenedor en modo independiente. Esto es solo por conveniencia.

    -p : expone los puertos HTTP y HTTPS de las aplicaciones

    --network: conecta el contenedor a la red Docker a la que está conectada la base de datos DB2

    --rm: elimina el contenedor cuando este se detiene. Esto es solo por conveniencia.

     


    ---

    El siguiente mensaje se muestra cuando se completan todos los pasos del script.

    ![](./images/media/build-container-done.png)

    ---

2. Verifique que el contenedor esté en funcionamiento.

    ```
    docker ps | grep pbw
    ```

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image60.png)

3. Desde el navegador, acceda a la aplicación PlantsByWebSphere que se ejecuta en el contenedor local

    ```
    http://server0.gym.lan:9080/PlantsByWebSphere
    ```

    ![Una captura de pantalla de una página web Descripción generada automáticamente](./images/media/image61.png)

    Y ver el catálogo de `Flowers` , que se recuperan de la base de datos.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image62.png)

4. `Stop` el contenedor PlantsByWebSphere

    ```
    docker stop pbw
    ```

**Consideraciones y recomendaciones:**

En la sección del laboratorio que acaba de completar, en la que se ejecuta la aplicación en un contenedor local, actualizamos el **archivo server.xml** agregando los valores de datos confidenciales para el acceso a la base de datos. Luego, incorporamos esa configuración a la imagen del contenedor.

Sin embargo, una gran parte del valor de las imágenes de contenedores es que son **`immutable`** . No importa dónde tomes e implementes la imagen, el sistema operativo, el entorno de ejecución, el nivel de parche de seguridad, etc. serán los mismos. Esto te brinda una gran reproducibilidad y te aleja del clásico problema de **"¡pero a mí me funciona!"** .

Perdemos gran parte de este valor si incorporamos la configuración con la imagen, ya que tendremos que producir una imagen para cada nueva configuración.

En lugar de utilizar exactamente la misma imagen en cada uno de sus entornos de desarrollo, prueba y producción, necesitaría utilizar imágenes diferentes.

---

**Nunca** es una buena práctica codificar de forma fija la configuración en la imagen.

---

**Recomendación:**

En la siguiente sección, veremos cómo el paquete de migración lo ayuda a administrar esta configuración fácilmente en todos sus entornos y cómo simplificará la implementación en su clúster de OpenShift. El paquete de migración utiliza `Kustomize` para ayudar a lograr esto.

## 3.2 Explore cómo se utiliza Kustomize en el paquete de migración

**`kustomize`** es una forma sencilla de administrar la configuración en todas sus diferentes implementaciones y entornos sin la necesidad de plantillas.

Al usar **`overlays`** , puede separar su información de configuración básica (puertos, nombres, hosts, etc.) de sus datos confidenciales (nombres de usuario, contraseñas, etc.) que probablemente cambien en cada implementación.

Cada artefacto **personalizado** es `YAML` simple y se puede validar y procesar de manera estandarizada. Además, ¡los hace muy legibles para humanos! Está integrado de forma nativa en **kubectl** y el **cliente OpenShift** .

1. Desde una **ventana `Terminal`** , enumere el contenido de la carpeta **`deploy`** del paquete de migración

    ```
    ls /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy
    ```

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image63.png)

    a. Hay dos carpetas.

    - **`k8s:`** contiene archivos para acelerar la implementación en **Kubernetes** . Estos ayudan a crear rutas, servicios e implementaciones. En este laboratorio, nos centraremos en la implementación en OpenShift con **kustomize** .

    - **`kustomize:`** contiene los archivos para la implementación mediante **kustomize** .

2. Vaya a la carpeta `Kustomize` ubicada debajo del directorio `deploy`

    ```
    cd /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize
    ```

3. Examine la estructura de la carpeta **kustomize** :

    ```
    ls -R
    ```

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image64.png)

    **Base:**

    - Contiene el **`Kustomization.yaml`** que describe los recursos administrados por Kustomize

    - contiene la **`application.cr-yaml`** Este es el **archivo de recursos personalizado del operador WebSphere Liberty** que ejecutará la implementación y montará los secretos y la configuración necesarios que se crean a partir del archivo secreto y el archivo de mapa de configuración.

    **Superposiciones:**

    - Contiene todas sus diferentes configuraciones de implementación.

    - En este caso, solo se ha creado una única implementación para sus sistemas **`dev`** .

    **Superposiciones/dev**

    - El `config map yaml file` contendrá todos los datos no confidenciales específicos de la aplicación que se crearán como configMaps en OpenShift.

    - El `secret yaml file` contendrá todos los datos confidenciales específicos de su aplicación que se crearán como secretos en OpenShift.

    - Opcionalmente, contiene archivos yaml adicionales que anulan la configuración en la configuración `base` .

    ---

    **Nota:** En la siguiente sección usaremos esta estructura personalizada para implementar la imagen de su aplicación.

    Puede encontrar más información sobre **kustomize** en [http://kustomize.io](http://kustomize.io)

    ---

## 3.3 Implementar en OpenShift con configuración

A partir de las secciones anteriores del laboratorio, tienes construida una `immutable container image` .

En esta sección, enviará esa imagen a un registro de imágenes y luego la implementará en el proyecto “ **dev** ” de OpenShift, junto con su configuración, con un solo comando, utilizando el `WebSphere Liberty Operator` y los artefactos de implementación generados en el `migration bundle` .

### 3.3.1 Implementar la aplicación en el entorno “dev” de OpenShift

La aplicación utiliza una base de datos DB2 que contiene un catálogo de elementos utilizados para el entorno "dev".

---

**Nota:**
 Después de implementar la aplicación en el entorno **de desarrollo** en OpenShift, explorará y examinará los artefactos yaml de Liberty Operator y Kustomize en el paquete de migración, que se utilizan para implementar la aplicación.

---

La implementación de la aplicación PlantsByWebSphere y su configuración en OpenShift se logra fácilmente utilizando un solo comando.

Sin embargo, en este laboratorio, antes de poder implementar la aplicación en OpenShift, la imagen del contenedor debe etiquetarse correctamente y enviarse a un registro de imágenes. En este laboratorio, utilizamos el registro interno de OpenShift.

Es necesario realizar los siguientes pasos, que se realizarán mediante un `convemience script` que se proporciona para este laboratorio.

- Etiquetar la imagen del contenedor para enviarla al Registro interno de OpenShift

- Iniciar sesión en OpenShift

- Crea el proyecto “dev” donde se implementará la aplicación

- Inicie sesión en el registro interno de OpenShift

- Insertar la imagen del contenedor en el registro

- Implementar la aplicación y su configuración en el proyecto “dev” de OpenShift

Para agilizar la ejecución de los pasos anteriores, hemos proporcionado un `convenience shell script` que realiza los pasos.

Ejecutará el script, que le solicitará que presione la tecla **`Enter`** antes de ejecutar cada uno de los comandos del script. Esto le permite examinar el comando antes de que se ejecute, evitando así tener que realizar actividades rutinarias de copiar y pegar en el laboratorio.

1. Ejecute el siguiente comando para etiquetar y enviar la imagen del contenedor PlantsByWebSphere al registro interno de OpenShift e implementar la aplicación en el proyecto “dev”.

    ```
     cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

     ./dev-deploy-pbw-ocp.sh -i
    ```

    **Nota:** El parámetro `-i` le indica al script que se ejecute en modo interactivo y le solicita que presione la tecla **`Enter`** para ejecutar el comando mostrado.

    ---

    **Nota:** El script también realiza cualquier tarea de limpieza necesaria, en caso de que necesite volver a ejecutar el script por alguna razón desconocida.

    ---

        <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Para referencia: Las capturas de pantalla de cada paso del script "dev-deploy-pbw-ocp"</summary>


    <br>


    **Paso 1:** Etiquete la imagen del contenedor PlantsByWebSphere para enviarla al registro interno de OpenShift

    ![Un libro blanco con texto negro Descripción generada automáticamente](./images/media/image65.png)

    **Paso 2:** Iniciar sesión en OpenShift

    ![Un libro blanco con texto negro Descripción generada automáticamente](./images/media/image66.png)

    **Paso 3:** Crea el proyecto 'dev' en OpenShift, donde se implementará la aplicación

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image67.png)

    **Paso 4:** Cambie al proyecto 'dev', donde se ejecutarán los comandos OCP posteriores en el script.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image68.png)

    **Paso 5:** Inicie sesión en el registro interno de OpenShift para que podamos enviar la imagen PlantsByWebSphere.

    ![Primer plano de la pantalla de una computadora Descripción generada automáticamente](./images/media/image69.png)

    **Paso 6:** Envíe la imagen del contenedor PlantsByWebSphere al registro de imágenes interno de OpenShift.

    ![Una captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image70.png)

    **Paso 7:** Enumere el flujo de imágenes y verifique que esté disponible.

    ![Primer plano de la pantalla de una computadora Descripción generada automáticamente](./images/media/image71.png)

    **Paso 8:** Implemente la aplicación PlantsByWebSphere y su configuración en el proyecto 'dev'.

    ![Una captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image72.png)

    **Paso 9:** Enumere la nueva 'implementación' y verifique que esté disponible.

    ![Una captura de pantalla de un programa de computadora Descripción generada automáticamente](./images/media/image73.png)

    **Paso 10:** Enumere el 'pod' de PlantsByWebSphere y verifique que esté listo.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image74.png)

    **Paso 11:** Muestra la 'ruta' a la aplicación PlantsByWebSphere que se ejecuta en el proyecto 'dev'.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image75.png)

      


    ---

    Cuando se complete el script, verás la **ruta PlantsByWebSphere** .

    ![Un primer plano de un código de computadora Descripción generada automáticamente](./images/media/image76.png)

    ---

2. Desde el navegador, navegue hasta la ruta PlantsByWebSphere para la implementación `dev` :

    ```
     http://plantsbywebsphereee6-dev.apps.ocp.ibm.edu/PlantsByWebSphere
    ```

    Opcionalmente, puede **`Right-mouse click`** en la **`PlantsByWebSphere URL`** en la ventana de la terminal y seleccionar **`open Link`** . Esto iniciará la aplicación en el navegador web.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image77.png)

3. Se muestra la página principal de PlantsByWebSphere.

    ![Página web de un jardín Descripción generada automáticamente](./images/media/image78.png)

4. Haga clic en la categoría **`Trees`** y vea los árboles que están cargados en la base de datos del entorno 'dev'.

    Este catálogo de árboles se recuperó de la base de datos DB2 en el entorno `dev` .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image79.png)

### 3.3.2 Revise la configuración de superposición “dev” que se utilizó para implementar la aplicación y la configuración

El directorio **`overlays/dev`** contiene una configuración específica y única para la implementación **`dev`** .

1. Examinar y comprender cómo configurar los datos confidenciales como secretos de Kubernetes

    a. Desde una ventana **`Terminal`** , navegue hasta el archivo yaml 'secrets' en la carpeta 'dev'overlay en el paquete de migración

    ```
     clear
     
     cd /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize

     cat overlays/dev/plantsbywebsphereee6-secret.yaml
    ```

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image80.png)

    Tenga en cuenta que ya insertamos el `user` y `password` de la base de datos DB2 en el archivo yaml que configura los datos confidenciales en un secreto de Kubernetes.

    **Nota:** Los datos de este archivo deben estar codificados en bese64.

    b. `How did we encode the values?`

    Para actualizar el paquete de migración con los datos confidenciales, **codificamos los valores en Base64** como se ilustra a continuación.

    - Codificamos los siguientes valores:

        ---

        - usuario: **db2inst1**

            eco db2inst1 | base64

        El valor codificado es: **ZGIyaW5zdDEK**

        ---

        - Contraseña: **db2inst1-pwd**

            eco db2inst1-pwd | base64

        El valor codificado es: **ZGIyaW5zdDEtcHdkCg==**

        ---

2. Desde una ventana **de Terminal** , examine el **`application-cr.yaml`**

    **`application-cr.yaml`** es el recurso personalizado de WebSphere Liberty que se utiliza para implementar la aplicación PlantsByWebSphere.

    ```
    cd /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize

    gedit base/application-cr.yaml
    ```

    a. En este laboratorio, optamos por anular la imagen de la aplicación, ya que enviaremos la misma imagen inmutable a diferentes espacios de nombres en OpenShift.

    Por lo tanto, siguiendo las reglas de Kustomize, la definición **de applicationImage** en application-cr.yaml está “en blanco”. Se reemplaza en overlays/dev.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image81.png)

    b. Las referencias `secret` y `configmap` incorporan la configuración del servidor como `environment variables` .

    ![Una captura de pantalla de un código de computadora Descripción generada automáticamente](./images/media/image82.png)

    c. Para este laboratorio, no tenemos habilitada la seguridad a nivel de transporte.

    ![](./images/media/image83.png)

    d. Debemos aceptar la licencia.

    ![](./images/media/image84.png)

    e. **`Close`** el editor `gedit` . **¡NO GUARDE NINGÚN CAMBIO EN EL ARCHIVO!**

3. Desde una ventana **de Terminal** , examine el `config map` para la implementación `dev`

    El **`plantsbywebsphereee6-configmap.yaml`** contiene las variables definidas en `server.xml` .

    Todos los valores definidos en el **configMap** se crean como variables de entorno en el contenedor. Estas variables de entorno anulan cualquier valor predeterminado que se haya establecido en el server.xml.

    ```
    cd /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize

    gedit overlays/dev/plantsbywebsphereee6-configmap.yaml
    ```

    a. Esta configuración reemplaza el **nombre del servidor de base de datos.** Hace referencia al servidor de base de datos de la aplicación para el entorno “ **dev** ”.

    ![](./images/media/image85.png)

    b. **`Close`** el editor “ **gedit** ”. **¡NO GUARDE NINGÚN CAMBIO EN EL ARCHIVO!**

### 3.3.3 Explorar el operador WebSphere Liberty en OpenShift

En esta sección, echará un vistazo al **operador WebSphere Liberty** en la consola OpenShift para ver qué se ha implementado.

1. Iniciar sesión en la consola OpenShift

    a. Abra una nueva pestaña en el navegador web.

    b. Haga clic en el marcador **`OpenShift Console`** en la barra de herramientas de marcadores.

    c. Credenciales de inicio de sesión:

    - Nombre de usuario: `ocadmin`

    - Contraseña: `ibmrhocp`

2. Ver el `Operator` **IBM WebSphere Liberty**

    a. Haga clic en **`Operators > Installed Operators`** en el menú de la izquierda.

    b. Escriba **`Liberty`** en el filtro

    c. Haga clic en **`IBM WebSphere Liberty`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image86.png)

3. Ver la `Deployment` **de WebSphereLibertyApplication**

    a. Haga clic en la pestaña llamada **`WebSphereLibertyApplication`**

    ![Un círculo verde con texto Descripción generado automáticamente](./images/media/image87.png)

    b. Verá la aplicación **plantsbywebsphereee6** incluida en el espacio de nombres 'dev'

    b. Haga clic en el enlace llamado **`plantsbywebsphereee6`** debajo de la columna **Nombre**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image88.png)

    c. Seleccione la pestaña **`Resources`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image89.png)

    d. Seleccione el enlace para la **`Deployment`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image90.png)

    e. La implementación de PlantsByWebSphere tiene un `pod` en ejecución

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image91.png)

4. Ver la **`Route`** WebSphereLibertyApplication

    a. Regresar a la página **del operador de plantsbywebsphereee6**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image92.png)

    b. Haga clic en la pestaña **`Resources`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image89.png)

    c. Seleccione el enlace de **Tipo: `route`**

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image93.png)

    d. Haga clic en el enlace **`Location`** para ver la ruta.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image94.png)

    e. La página **Bienvenido a Liberty** se muestra en una nueva pestaña del navegador.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image95.png)

    f. Agregue la raíz de contexto ' **PlantsByWebSphere'** para acceder a la aplicación PlantsByWebSphere

    ```
    http://plantsbywebsphereee6-dev.apps.ocp.ibm.edu/PlantsByWebSphere
    ```

# Resumen

---

**¡Felicidades!**

**Ha completado con éxito los objetivos de aprendizaje básicos en el laboratorio.**

---

En este laboratorio, aprendió a implementar aplicaciones en WebSphere Liberty utilizando el operador WebSphere Liberty y los artefactos de implementación producidos por Transformation Advisor en su paquete de migración.

Exploraste las opciones de implementación:

- Liberty, que se ejecuta localmente
- La libertad como imagen que se ejecuta en un contenedor local
- Libertad como imagen ejecutándose en OpenShift.

Aprendió cómo configurar fácilmente implementaciones en OpenShift para permitir que se implemente la misma imagen inmutable para diferentes configuraciones, como implementaciones de entornos "de desarrollo" y "de prueba".

Aprendió algunas de las formas prácticas en que puede proteger sus datos de configuración.

Exploró los detalles y los informes de la evaluación de PlantsByWebSphere en Transformation Advisor.

---

Puede continuar su viaje de aprendizaje con las secciones opcionales del laboratorio.

---

# Secciones opcionales:

{::opciones parse_block_html="verdadero" /}

  <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Sección 3.4 GitOps y gestión de configuración</summary>

<br>

## 3.4 GitOps y gestión de configuración

En este punto, tenemos dos elementos distintos para la implementación: la **imagen de la aplicación** y la **configuración de esa imagen para cada entorno potencial** . En el modelo de GitOps, estos dos elementos se almacenan en repositorios de Git separados.

Puede distribuir la configuración en diferentes repositorios para cada entorno. Este enfoque le permite tratar la configuración como si fuera código y aplicar técnicas de desarrollo de código estándar, como solicitudes de incorporación de cambios, revisión de código y mantenimiento de un historial de auditoría completo. Este enfoque ofrece excelentes controles y supervisión de los cambios de configuración para sus clústeres de OpenShift, especialmente a medida que promueve cambios en sus entornos.

![Un fondo negro con flechas y puntos blancos Descripción generada automáticamente](./images/media/image105.png)

La ilustración anterior proviene de un excelente artículo sobre GitOps en el mundo real: [https://developer.ibm.com/blogs/gitops-best-practices-for-the-real-world/](https://developer.ibm.com/blogs/gitops-best-practices-for-the-real-world/)

No cubriremos todo GitOps en detalle en este laboratorio; sin embargo, hay dos áreas relevantes que vale la pena mencionar brevemente.

**Desviación de configuración**

Con el enfoque de GitOps, existe el riesgo de que la configuración implementada para el clúster de OpenShift no coincida con lo que está en el repositorio de Git. Alguien podría haber actualizado el clúster de OpenShift directamente, por ejemplo. Hay varias herramientas para gestionar esta desviación de la configuración, y ArgoCD es una herramienta común para abordar este problema. Se puede configurar para que se sincronice de forma manual o automática, de modo que cualquier cambio realizado directamente en el clúster se revierta a lo que está en el repositorio de Git.

El operador GitOps de OpenShift se puede utilizar para instalar ArgoCD en su clúster OpenShift. Puede encontrar más detalles sobre cómo hacerlo aquí: [https://developer.ibm.com/tutorials/deploy-open-liberty-applications-with-gitops/](https://developer.ibm.com/tutorials/deploy-open-liberty-applications-with-gitops/)

**Asegurando secretos**

Incluso con controles y acceso restringido a los repositorios de Git, no se considera una buena práctica almacenar los datos confidenciales en texto sin formato (es decir, los valores ingresados en los archivos secretos). En general, existen dos enfoques que puede adoptar para abordar esto: cifrar los valores o usar una referencia:

- **Cifrar los valores** : en este enfoque, el valor del archivo secreto se cifra cuando se envía al repositorio de Git. Durante la implementación en OpenShift, hay un paso adicional en el que se descifran los valores secretos. `SealedSecrets` es una implementación de este enfoque.

- **Utilizar una referencia** : en este caso, los secretos y sus valores se almacenan en un administrador de secretos. Durante la implementación, se proporciona una referencia al administrador de secretos, que luego montará el secreto en el contenedor implementado. `HashiCorp Vault` es una implementación común de este enfoque.

En ambos casos, hay trabajo adicional que realizar además de lo que se ha cubierto en este laboratorio. Sin embargo, los archivos personalizados que se proporcionan en el paquete de migración le brindan un buen punto de partida para identificar los secretos y proporcionar una salida estándar que se puede transformar para adaptarse al enfoque seleccionado.




  <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Sección 4: (Opcional) Promocionar PlantsByWebSphere al entorno de "preparación" en OpenShift</summary>

<br>

## 4.1 Volver a implementar PlantsByWebSphere en el entorno de "ensayo" en OpenShift

Ha implementado la aplicación PlantsByWebSphere con su configuración en un entorno 'dev', utilizando un único comando kustomize.

En esta sección, implementará nuevamente la misma aplicación, con una configuración diferente, en un entorno de "preparación", utilizando un solo comando kustomize con una configuración de superposición diferente denominada "preparación".

**Para ello debemos hacer lo siguiente:**

- Copia los archivos yaml de superposiciones "dev" en una nueva carpeta de superposiciones. La llamaremos "staging".

- Actualice la configuración específica del entorno en la nueva carpeta de superposiciones 'staging'.

---

**Nota:**

Para esta sección del laboratorio, la aplicación PlantsByWebSphere que implemente en el entorno de "preparación" accederá a una base de datos DB2 diferente a la que se utilizó en el entorno de "desarrollo".

Es habitual utilizar diferentes bases de datos a medida que la aplicación se promueve a entornos superiores que admiten diferentes fases de prueba hasta producción.

---

### 4.1.1 Configurar y examinar la configuración de implementación de 'preparación'

En este laboratorio, la base de datos 'staging' tiene un conjunto diferente de 'árboles' cargados en el catálogo, en comparación con la base de datos 'dev'.

Si bien en un entorno real las diferentes bases de datos pueden utilizar diferentes credenciales de acceso, en este entorno de laboratorio las credenciales siguen siendo las mismas que las de la base de datos "dev".

**¿Cuáles son los cambios de configuración entre los entornos 'dev' y 'staging' en este escenario de laboratorio?**

- El **nombre de host** **del servidor** de base de datos es diferente entre 'dev' y 'staging'.

**¿Qué permanece igual entre la implementación 'dev' y la 'staging'?**

- La misma imagen de contenedor inmutable para la aplicación PlantsByWebSphere se utiliza en 'dev' y 'staging'

- Seguimos utilizando el mismo nombre de base de datos: PBW

- Seguimos utilizando el mismo puerto de base de datos: 50000

- Seguimos utilizando las mismas credenciales de acceso a la base de datos.

Los valores del **nombre de la base de datos, el puerto y el host** se almacenan en un archivo YAML **de mapa de configuración** . Por lo tanto, el archivo YAML de mapa de configuración en la carpeta superpuesta "staging" será diferente del archivo de mapa de configuración utilizado en la carpeta superpuesta "dev".

Esto significa que para implementar la aplicación PlantsByWebSphere en el entorno de "preparación", todo lo que se debe hacer es actualizar el `DB2 hostname` en el archivo YAML del `config map` ubicado en la carpeta de superposiciones de "preparación". Luego, ejecute el comando único para implementar la aplicación PlantsByWebSphere en el entorno de preparación.

**Ahora estamos listos para comenzar:**

- Copia los archivos yaml de superposiciones "dev" en una nueva carpeta de superposiciones. La llamaremos "staging".

- Actualice la configuración específica del entorno en la nueva carpeta de superposiciones 'staging'.

Para agilizar la ejecución de los pasos anteriores, proporcionamos un `convenience shell script` que realiza estos pasos.

`run the script` para configurar la superposición de ensayo, mientras evitas tener que realizar actividades rutinarias de copiar y pegar en el laboratorio.

Después de ejecutar el script, revisaremos los cambios que se realizaron en el archivo yaml del mapa de configuración.

1. Ejecute los siguientes comandos para configurar la carpeta `overlays/staging` con el mapa de configuración actualizado.

    ```
    cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

    ./kustomize-staging.sh
    ```

    **Nota:** El script también realiza cualquier tarea de limpieza necesaria, en caso de que necesite volver a ejecutar el script por alguna razón desconocida.

    ![Un primer plano de un logotipo Descripción generada automáticamente](./images/media/image96.png)

2. Examinar la superposición **de puesta en escena**

    ```
    ls /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize/overlays
    ```

    El contenido se copió "tal cual" desde el directorio de superposición "dev"

    ![Captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image97.png)

3. El directorio de superposición ' **staging'** contiene los mismos 4 archivos yaml que la superposición 'dev'

    ```
    ls /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize/overlays/staging
    ```

    ![](./images/media/image98.png)

4. El archivo **plantsbywebspheree6-configmap.yaml** contiene la configuración específica del entorno de ' **staging'** .

    Específicamente, el “ **DataSource serverName** ” para la base de datos DB2 se ha actualizado al host de la base de datos del entorno de 'provisionamiento'

    ```
    gedit /home/techzone/Student/labs/appmod/pbw-bundle-complete/deploy/kustomize/overlays/staging/plantsbywebsphereee6-configmap.yaml
    ```

    ![](./images/media/image99.png)

5. **`Close`** el editor "gedit". **¡NO GUARDE NINGÚN CAMBIO EN EL ARCHIVO!**

### 4.1.2 Implementar PlantsByWebSphere en un entorno de ensayo

En esta sección, ejecutará los mismos pasos para implementar y configurar la aplicación en el entorno de prueba que se utilizaron en desarrollo.

La diferencia son los deltas de configuración que examinó en las carpetas de superposición/preparación en el paquete de migración.

1. Ejecute el siguiente `convenience shell script` para implementar la aplicación PlantsByWebSphere en el entorno de " **preparación"** .

    **Nota:** El script realiza los mismos pasos que la implementación en el entorno 'dev'.

    ---

    El script realiza cualquier tarea de limpieza necesaria en caso de que necesite volver a ejecutar el script por alguna razón desconocida.

    ---

    ```
    cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts

    ./staging-deploy-pbw-ocp.sh
    ```

    Observe que en **`step 8`** del script, el comando para implementar la aplicación utiliza la configuración de la superposición `staging` , como se ilustra a continuación.

    ![Un primer plano de un código de computadora Descripción generada automáticamente](./images/media/image100.png)

Cuando se completa el script, se muestra la URL (ruta) a la aplicación PlantsByWebSphere que se ejecuta en el entorno de prueba.

![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image101.png)

1. Abra una nueva pestaña del navegador y vaya a esa URL:

    ```
    http://plantsbywebsphereee6-staging.apps.ocp.ibm.edu/PlantsByWebSphere
    ```

    ![Una pantalla verde con una pérgola Descripción generada automáticamente](./images/media/image102.png)

2. En la aplicación, haga clic en la categoría “ **Árboles** ”. Esta es la lista de árboles en la base de datos de “ **preparación** ”.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image103.png)

3. Puede consultar el catálogo de árboles en el entorno ' **dev'** y observar que los árboles tienen nombres diferentes en el entorno de prueba.

    a. Abra una nueva pestaña del navegador y vaya a la aplicación que se ejecuta en el entorno `dev` .

    ```
    http://plantsbywebsphereee6-dev.apps.ocp.ibm.edu/PlantsByWebSphere
    ```

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image104.png)

    


  <summary><b><font color="dodgerblue">Haga clic para expandir:</font></b> Sección 5: (Opcional) Exploración de la aplicación PlantsByWebSphere en el espacio de trabajo de prueba de concepto</summary>

<br>

## 5.1 Exploración de la aplicación PlantsByWebSphere en el espacio de trabajo de prueba de concepto

IBM Cloud Transformation Advisor (TA) analiza los datos de configuración y aplicaciones recopilados de los entornos de servidores de aplicaciones Java. TA ofrece información valiosa que ayuda a las organizaciones a planificar e implementar un proceso de modernización hacia la plataforma de contenedores base WebSphere Liberty y Kubernetes para las aplicaciones Java existentes.

TA proporciona recomendaciones para la modernización que incluyen y consideran la complejidad de la aplicación, las dependencias, los problemas potenciales y el esfuerzo de desarrollo estimado.

![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image106.png)

1. En una ventana del navegador, regrese a Transformation Advisor utilizando la siguiente URL:

    ```
     http://localhost:3000
    ```

2. Abra el espacio de trabajo `proof-of-concept` , si aún no está abierto

    ![](./images/media/ta-proof-of-concept.png)

3. Desplácese hacia abajo hasta la sección `Java applications` y observe el resumen de alto nivel de la evaluación de la aplicación " **plantsbywebsphereee6.ear** ".

- Complejidad

- Asuntos

- Cambios de código necesarios

- Costo de la aplicación (en días de desarrollo)

    ![](./images/media/image107.png)

Este espacio de trabajo tiene un único nombre de aplicación: `plantsbywebsphereee6.ear` . Se resume como `simple` de modernizar a WebSphere Liberty.

***Valores de complejidad y sus significados:***

**`Simple`** : la aplicación está lista para su implementación, no se requiere acceso al código fuente.

- El valor de complejidad simple generalmente representa alrededor del 20% de las aplicaciones.

**`Moderate`** : se requieren cambios de código antes de la implementación; sin embargo, estos cambios de código son bien conocidos y se proporciona ayuda específica para cada problema para ayudar a resolverlo.

- El valor de complejidad moderada generalmente representa alrededor del 80% de las aplicaciones.

**`Complex`** : la aplicación utiliza una tecnología que no tiene un equivalente directo en el nuevo entorno de ejecución y será necesario adoptar un nuevo enfoque. Según nuestra experiencia, solo 1 de cada 6 clientes tiene este tipo de aplicaciones.

- El valor de complejidad complejo generalmente representa menos del 1% de las aplicaciones.

1. Haga clic en **plantsByWebSphereee6.ear** que ampliará el análisis.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image108.png)

    a. Desplácese hacia abajo y observe los grupos o categorías de análisis disponibles.

    - **La complejidad manda**

    - **Cambios de código necesarios**

    - **Detalles del problema**

        - Crítico

        - Advertencia

        - Informativo

    - **Informes adicionales**

        - Informe sobre tecnología

        - Informe de inventario

        - Informes de análisis

2. Vea el **`Analysis Report`** para obtener información más detallada sobre la aplicación

    **Informe de análisis**

    El **Informe de análisis de migración detallado** analiza en profundidad el destino de migración preferido para ayudarlo a comprender cualquier problema de migración, como API obsoletas o eliminadas, diferencias de versiones de Java SE y diferencias de comportamiento de Java EE.

    a. Haga clic en el enlace **`Analysis report`** .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image109.png)

    b. El **informe de análisis** se abrirá en una nueva pestaña del navegador.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image110.png)

    c. Tenga en cuenta el **objetivo**

    - Dado que la **fuente** era el tradicional WebSphere v8.5.5, que es Java EE 6, el destino también es EE6.

    - Dado que la fuente ejecutaba Java SE 8, el destino también está configurado en Java SE 8.

    De forma predeterminada, el asesor de transformación recomienda el primer paso de modernización a Liberty que requiere la mínima cantidad de cambio y esfuerzo.

    Puede utilizar el escáner binario con la opción `--ta` si desea evaluar el esfuerzo de modernización a versiones más nuevas de Java, Java EE o Jakarta EE. Al ejecutar el escáner binario con la opción `-ta` se genera un archivo que se puede cargar en Transformation Advisor para su análisis.

    d. Haga clic en la etiqueta **`Information`** para revisar los detalles de este artículo.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image112.png)

    Las reglas **de información** brindan consideraciones relacionadas con la migración. Tenga en cuenta estas reglas durante las pruebas. Muchas de estas reglas se relacionan con la conectividad con otros recursos que deben tenerse en cuenta durante la migración.

    e. Puede revisar las reglas de información para la migración de PlantsByWebSphere.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image113.png)

3. Regrese a la pestaña del navegador **Cloud Transformation Advisor** que muestra la página **de detalles de plantsbywebsphereee6.ear** . Luego, haga clic en **`Inventory Report`** , que se abrirá en una nueva pestaña del navegador.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image114.png)

    a. Desplácese hacia abajo y revise el Informe de inventario, que es especialmente útil en aplicaciones más grandes.

    El **informe de inventario** proporciona un inventario de alto nivel del contenido y la estructura de cada aplicación, además de información sobre posibles problemas de implementación y consideraciones de rendimiento.

    ![](./images/media/image115.png)

    b. Desplácese hasta la parte inferior del **informe de Inventario** y localice **`the Contained Archives`** .

    El informe de inventario proporciona información valiosa sobre los archivos jar de utilidades que contiene la aplicación. También proporciona el nombre del "paquete" del archivo jar de utilidades. Esto es extremadamente valioso para ayudar a determinar qué utilidades de <sup>terceros</sup> utiliza la aplicación.

    c. Observe que la aplicación PlantsByWebSphere contiene un archivo jar de utilidad llamado **'pbw-lib'jar** '. El paquete de archivo es "ibm.com.websphere", lo que indica que NO es un archivo jar de utilidad de <sup>terceros</sup> .

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image116.png)

    d. Regrese a la pestaña del navegador **Cloud Transformation Advisor** que muestra la página **de detalles de plantsbywebsphereee6.ear** . Luego, haga clic en **`Technology report`** , que se abrirá en una nueva pestaña del navegador.

    El **informe de tecnología** proporciona detalles sobre qué ediciones de Liberty admiten las tecnologías utilizadas por las aplicaciones.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image117.png)

    a. Desplácese hacia abajo hasta el Informe de tecnología, que puede ayudar a evaluar rápidamente qué tecnologías de API de Java EE utiliza la aplicación PlantsByWebSphere y qué ediciones de las API de Liberty están disponibles.

    ![Una captura de pantalla de una computadora Descripción generada automáticamente](./images/media/image118.png)

    b. Como puede ver en el informe anterior, las API utilizadas en la aplicación PlantsByWebSphere están disponibles en TODAS las ediciones de Liberty.




{::opciones parse_block_html="falso" /}

**===== FIN DEL LABORATORIO =====**

[Regresar a la página principal de laboratorios](../README.md)
