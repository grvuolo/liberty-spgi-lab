# Laboratorio práctico Open Liberty InstantOn

[Regresar a la página principal de laboratorios](../README.md)

En este laboratorio, creará una aplicación Open Liberty simple y la ejecutará en un contenedor, que luego podrá implementar de múltiples maneras: localmente y en Kubernetes.

La aplicación se ejecutará en 2 modos: estándar y con InstantOn. Podrás comprobar la mejora significativa en el tiempo de inicio que proporciona InstantOn.

Este laboratorio lo guiará a través de los pasos necesarios para crear imágenes InstantOn, así como también cómo implementar imágenes en OpenShift Cloud Platform.

Tenga en cuenta que en muchos de los comandos que se enumeran a continuación, hemos incluido un archivo para ejecutar el comando. Puede optar por escribir los comandos usted mismo o simplemente ejecutar el script.

Este laboratorio requiere que inicie al menos una sesión de terminal e inicie el navegador Firefox.

Como parte de este laboratorio, es necesario instalar y configurar el servicio Knative. Normalmente, esto lo realizará el instructor antes del laboratorio y estará disponible para que todos lo utilicen. Si usted es el instructor o si desea ver los pasos necesarios, consulte las [instrucciones de configuración de Knative](https://github.com/rhagarty/techxchange-knative-setup) .

# Pasos:

1. [Configuración inicial del laboratorio](#1-initial-lab-setup)
2. [Construya y ejecute la aplicación localmente](#2-build-and-run-the-application-locally)
3. [Envía las imágenes al registro de OpenShift](#3-push-the-images-to-the-openshift-registry)
4. [Mejorar el entorno de OpenShift Cloud Platform (OCP)](#4-enhance-the-openshift-cloud-platform-ocp-environment)
5. [Implementar las aplicaciones en OCP](#5-deploy-the-applications-to-ocp)

## 1. Configuración inicial del laboratorio

### Configuración de la máquina virtual InstantOn Lab

Reserve el laboratorio InstantOn en TechZone para configurar la VM.

### Configuración de la máquina virtual del servidor OCP

> **NOTA** : No necesitará realizar este paso si el organizador de su evento ha configurado el servidor OCP para usted.

Siga las instrucciones mencionadas en la descripción del laboratorio InstantOn para configurar el servidor OCP.

### Iniciar sesión como root

Después de aprovisionar la máquina virtual InstantOn Lab y asegurarse de que esté lista, utilice el enlace NoVNC proporcionado para acceder al escritorio del laboratorio. Luego, inicie sesión como usuario raíz desde la terminal del laboratorio.

```bash
su
```

Utilice como contraseña root: `IBMDem0s!`

### Upgrade OpenJDK

```bash
wget https://github.com/ibmruntimes/semeru21-binaries/releases/download/jdk-21.0.2%2B13_openj9-0.43.0/ibm-semeru-open-21-jdk-21.0.2.13_0.43.0-1.x86_64.rpm
yum localinstall ibm-semeru-open-21-jdk-21.0.2.13_0.43.0-1.x86_64.rpm 
alternatives --config java
vi ~/.bashrc
	export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-21.0.2.0.13-1.el9.x86_64
	PATH=$PATH:JAVA_HOME/bin
source ~/.bashrc
```

### Clonar la aplicación desde GitHub

```bash
cd /home/ibmuser/Lab-InstantOn
git clone https://github.com/grvuolo/liberty-spgi-lab.git
cd liberty-spgi-lab/141-Liberty-InstantOn-Serverless
```

> **NOTA** : Si prefiere utilizar un IDE para ver el proyecto, puede ejecutar VSCode con privilegios de administrador usando el siguiente comando:
>
> ```bash
> sudo code --no-sandbox --user-data-dir /home/techzone
> ```

### Inicie sesión en la consola OpenShift, utilizando la siguiente URL:

Se ha aprovisionado un clúster OpenShist compartido en la siguiente URL: https://console-openshift-console.apps.67a3346aadf42b5f8ef28cdc.eu1.techzone.ibm.com/

Ir a `IBM Demo`

nombre de usuario: usuario[x] contraseña: passw0rd

### Inicie sesión en la CLI de OpenShift [SI ES NECESARIO]

> **NOTA** : No necesitará realizar este paso si completó el laboratorio de Semeru Cloud Compiler. Para verificar, puede usar `oc whoami` para determinar si ya inició sesión en la CLI como `kubeadmin` .

Desde la interfaz de usuario de la consola OpenShift, haga clic en el nombre de usuario en la esquina superior derecha y seleccione `Copy login command` .

![inicio de sesión ocp-cli](images/ocp-cli-login.png)

Presione `Display Token` y copie el comando `Log in with this token` .

![token cli-ocp](images/ocp-cli-token.png)

Pegue el comando en la ventana de su terminal. Debería recibir un mensaje de confirmación que indique que ha iniciado sesión.

## 2. Construya y ejecute la aplicación localmente

### Empaquetar la aplicación

Primero asegúrese de estar en el directorio `liberty-spgi-lab/141-Liberty-InstantOn-Serverless` , luego ejecute `mvn package` para compilar la aplicación.

```bash
mvn package
```

### Cambiar docker a podman

```bash
mv /usr/bin/docker /usr/bin/docker_backup
cp /usr/bin/podman.backup /usr/bin/podman
```

### Construir la imagen de la aplicación

Ejecute el script proporcionado:

```bash
sudo ./build-local-without-instanton.sh
```

O escriba el siguiente comando:

```bash
sudo podman build -t dev.local/getting-started .
```

> **NOTA** : El Dockerfile utiliza una versión reducida de Java 21 Open Liberty UBI.
>
> `FROM icr.io/appcafe/open-liberty:kernel-slim-java21-openj9-ubi-minimal`
>
> Si encuentra algún problema al ejecutar este paso, consulte las [notas de solución de problemas](#error-when-building-the-application-imagewithout-instanton) proporcionadas.

### Ejecutar la aplicación en un contenedor

Ejecute el script proporcionado:

```bash
./run-local-without-instanton.sh
```

O escriba el siguiente comando:

```bash
podman run --name getting-started --rm -p 9080:9080 dev.local/getting-started
```

Cuando la aplicación esté lista, verás el siguiente mensaje:

```bash
[INFO] [AUDIT] CWWKF0011I: The server defaultServer is ready to run a smarter planet.
```

Tenga en cuenta el tiempo que tarda Open Liberty en informar que se ha iniciado (normalmente entre 3 y 5 segundos).

Pruebe la aplicación apuntando su navegador a http://localhost:9080/dev.

Para detener el contenedor en ejecución, presione `CTRL+C` en la sesión de línea de comandos donde ejecutó el comando `podman run` .

### Actualice el Dockerfile para utilizar InstantOn

Para convertir esta imagen para usar InstantOn, modifique el `Dockerfile` agregando la siguiente línea al final del archivo.

```bash
RUN checkpoint.sh afterAppStart
```

Puedes utilizar este comando:

```bash
echo RUN checkpoint.sh afterAppStart >> Dockerfile
```

Este comando realizará las siguientes acciones:

1. Ejecutar la aplicación
2. Tome un punto de control después de que se haya cargado el código de la aplicación
3. Detener la aplicación

Tenga en cuenta que hay 2 opciones de puntos de control:

`beforeAppStart` : realiza un punto de control después de inspeccionar la aplicación y analizar las anotaciones de la aplicación, los metadatos, etc., pero ANTES de ejecutar cualquier código de la aplicación.

`afterAppStart` : realiza un punto de control después de ejecutar cualquier código de aplicación que deba ejecutarse antes de que la JVM pueda informar que la aplicación se ha iniciado y está lista para recibir solicitudes. Este punto de control no es adecuado si el código de inicio de la aplicación realiza alguna de las siguientes acciones:

- Acceder a un recurso remoto, como una base de datos
- Configuración de lectura que se espera que cambie cuando se implementa la aplicación
- Iniciar una transacción

### Cree la imagen de la aplicación con el punto de control InstantOn

Ejecute el script proporcionado:

```bash
sudo ./build-local-with-instanton.sh
```

O escriba el siguiente comando:

```bash
sudo podman build \
  -t dev.local/getting-started-instanton \
  --cap-add=CHECKPOINT_RESTORE \
  --cap-add=SYS_PTRACE\
  --cap-add=SETPCAP \
  --security-opt seccomp=unconfined .
```

> **IMPORTANTE** : Necesitamos agregar varias capacidades de Linux que se requieren cuando se crea la imagen del punto de control.

Debería ver lo siguiente en la salida de la compilación:

```bash
...
Performing checkpoint --at=afterAppStart
...
```

### Ejecute la aplicación InstantOn en un contenedor

Ejecute el script proporcionado:

```bash
./run-local-with-instanton.sh
```

O escriba el siguiente comando:

```bash
podman run \
 --rm \
 --cap-add=CHECKPOINT_RESTORE \
 --cap-add=SETPCAP \
 --security-opt seccomp=unconfined \
 -p 9080:9080 \
 dev.local/getting-started-instanton
```

> **IMPORTANTE** : Necesitamos agregar varias capacidades de Linux y opciones de seguridad para que el contenedor tenga los privilegios correctos al ejecutarse.

Cuando la aplicación esté lista, verás el siguiente mensaje:

```bash
[INFO] [AUDIT] CWWKF0011I: The server defaultServer is ready to run a smarter planet.
```

Tenga en cuenta el tiempo de inicio y compárelo con la versión sin InstantOn. Debería ver un tiempo de inicio en el rango de los 300 milisegundos, ¡una mejora de 10 veces!

Pruebe la aplicación apuntando su navegador a http://localhost:9080/dev.

Para detener el contenedor en ejecución, presione `CTRL+C` en la sesión de línea de comandos donde ejecutó el comando `podman run` .

## 3. Inserte las imágenes en el registro de OpenShift

### Crea el espacio de nombres y configúralo como predeterminado

> **NOTA** : Si está trabajando en un clúster compartido con otros, ya se ha creado un espacio de nombres para usted. Si su usuario es *user1* , entonces su espacio de nombres será *instantonlab-1* . Cambie [Su inicial] según corresponda.

```bash
export CURRENT_NS=user[User Number]-ns
```

> **NOTA** : su espacio de nombres ya ha sido creado

Debería ser el proyecto predeterminado (y el único al que tiene acceso), pero asegúrese de estar trabajando en ese espacio de nombres ejecutando:

```bash
oc project $CURRENT_NS
```

### Habilite la ruta de registro predeterminada en OpenShift para enviar imágenes a sus repositorios internos [no es necesario cuando se usa un OpenShift compartido]

> **NOTA** : No necesitará realizar este paso ya que el registro ya está expuesto

```bash
oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
```

### Inicie sesión en Podman en el servidor de registro OpenShift [SI ES NECESARIO]

> **NOTA** : este paso no es necesario ya que el instructor ya proporcionó el secreto y el acceso al registro está disponible con el inicio de sesión en OCP

Primero necesitamos obtener el `TOKEN` que podemos usar para obtener la contraseña para el registro.

```bash
oc get secrets -n openshift-image-registry | grep cluster-image-registry-operator-token
```

Tome nota del valor `TOKEN` , ya que deberá sustituirlo en el siguiente comando que establece la contraseña del registro.

```bash
export OCP_REGISTRY_PASSWORD=eyJhbGciOiJSUzI1NiIsImtpZCI6IlpwUVFqYVRFb1phdVhSUExPVmFrdXhjR1R3TmR0TU4yd216aU5UNHdEOEEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJvcGVuc2hpZnQtaW1hZ2UtcmVnaXN0cnkiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlY3JldC5uYW1lIjoiY2x1c3Rlci1pbWFnZS1yZWdpc3RyeS1vcGVyYXRvci10b2tlbi02ZDg2MiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJjbHVzdGVyLWltYWdlLXJlZ2lzdHJ5LW9wZXJhdG9yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiMDQwZDVhM2YtYjg1Ny00MWI2LTlhZDYtOTllOGI1YmY4ZTgwIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Om9wZW5zaGlmdC1pbWFnZS1yZWdpc3RyeTpjbHVzdGVyLWltYWdlLXJlZ2lzdHJ5LW9wZXJhdG9yIn0.lVf_JvbJapMfZPHog-HqUJa54pqbJwxztNZFk_7ckhPrds0yghGbl5_EhDusdC6ovHimEeWs8UGZn4f4xRhEDcu_5ELm4QKgMNeev3oIeM-9y2Xnk-YdY7XHGGRFXET6UpkoPd5HAud0hj6wI1moxXkPG75Vy6J98L_9wl4yYansZo3Y6UHXary6ZOvpB-Fnf1yJW5ujMgrdBWjW6-Z4IlTF7UeKt9HjNTKADTskmVe-ngR1yeRMQB313Ob2btiCNHyRKHj0dV8_xzK5gCLQoEdbcInUkjLs5pcyOxMQ8TnTNlR1vnUby1qbeneCs2Ibd2xLz-3nE55yEJV88MLIZw
```

Ahora configure el valor del host de registro de OpenShift.

```bash
export OCP_REGISTRY_HOST=default-route-openshift-image-registry.apps.67a3346aadf42b5f8ef28cdc.eu1.techzone.ibm.com
```

Finalmente, tenemos los valores necesarios para que podman inicie sesión en el servidor de registro OpenShift.

```bash
podman login -p $OCP_REGISTRY_PASSWORD -u user[number] $OCP_REGISTRY_HOST --tls-verify=false
```

### Etiqueta y envía las 2 imágenes de tu aplicación al registro de OpenShift

Utilice `podman images` para verificar sus 2 imágenes locales. Los nombres de las imágenes deben ser:

- dev.local/primeros pasos
- dev.local/introducción-instanton

Ahora etiquételos y envíelos al registro de OpenShift:

```bash
# base application image
podman tag dev.local/getting-started:latest $(oc registry info)/$(oc project -q)/getting-started:1.0-SNAPSHOT
podman push $(oc registry info)/$(oc project -q)/getting-started:1.0-SNAPSHOT --tls-verify=false
```

```bash
# InstantOn application image
podman tag dev.local/getting-started-instanton:latest $(oc registry info)/$(oc project -q)/getting-started-instanton:1.0-SNAPSHOT
podman push $(oc registry info)/$(oc project -q)/getting-started-instanton:1.0-SNAPSHOT --tls-verify=false
```

### Verifique que las imágenes se hayan enviado al repositorio de imágenes de OpenShift

```bash
oc get imagestream
```

## 4. Mejorar el entorno de OpenShift Cloud Platform (OCP)

> **NOTA** : este paso no es necesario ya que el operador de Liberty está disponible. Confíe en el instructor

Realice los siguientes pasos para mejorar OCP y administrar mejor los servicios de OCP, como Knative, que proporciona funcionalidad sin servidor o de escala a cero.

El operador Liberty proporciona recursos y configuraciones que facilitan la ejecución de aplicaciones Open Liberty en OCP. Para verificar si el operador Liberty está disponible en el servidor, utilice el siguiente comando:

```bash
oc get crd openlibertyapplications.apps.openliberty.io openlibertydumps.apps.openliberty.io openlibertytraces.apps.openliberty.io
```

Deberías ver el siguiente resultado:

![verificar-liberty-operator](images/verify-liberty-operator.png)

### Aplique el operador Liberty a su espacio de nombres [no es necesario cuando se utiliza un OpenShift compartido]

> **NOTA** : este paso no es necesario ya que el operador de Open Liberty ya está disponible para su namespace. Confíe en el instructor

```bash
OPERATOR_NAMESPACE=instantonlab-[Your initial]
WATCH_NAMESPACE=instantonlab-[Your initial]
##
curl -L https://raw.githubusercontent.com/OpenLiberty/open-liberty-operator/main/deploy/releases/1.3.1/kubectl/openliberty-app-operator.yaml \
      | sed -e "s/OPEN_LIBERTY_WATCH_NAMESPACE/${WATCH_NAMESPACE}/" \
      | oc apply -n ${OPERATOR_NAMESPACE} -f -
```

### Verificar la instalación del Administrador de certificados [no es necesario cuando se utiliza un OpenShift compartido]

El administrador de certificados agrega certificaciones y emisores de certificaciones como tipos de recursos a Kubernetes

> **NOTA** : este paso no es necesario ya que el operador de Open Liberty ya está disponible para su espacio de nombres. Confíe en el instructor

```bash
oc get deployments -n cert-manager
```

Deberías ver el siguiente resultado:

![verificar-certificado](images/verify-cert.png)

### Verifique que el operador sin servidor OpenShift esté instalado y listo

```bash
oc get csv
```

> **NOTA** : Si encuentra algún problema al ejecutar este paso, consulte las [notas de solución de problemas](#error-when-verifying-the-openshift-serverless-operator-is-installed-and-ready) proporcionadas.

Deberías ver el siguiente resultado:

![ocp sin servidor](images/ocp-serverless.png)

### Verifique que el servicio Knative esté listo [no es necesario cuando se usa un OpenShift compartido]

```bash
oc get knativeserving.operator.knative.dev/knative-serving -n knative-serving --template='{{range .status.conditions}}{{printf "%s=%s\n" .type .status}}{{end}}'
```

Si se ha configurado el servicio Knative y se ha agregado a la capacidad, su salida debería coincidir con lo siguiente:

![ocp-knative](images/ocp-knative.png)

> **NOTA** : Si el resultado no coincide con el esperado o si hay otros problemas, comuníquese con su instructor.
>
> Para obtener más información sobre la configuración del servicio Knative, consulte nuestras [instrucciones de configuración de Knative.](https://github.com/rhagarty/techxchange-knative-setup)

### Verifique que la función Knative ContainerSpec-AddCapabilities esté habilitada [no es necesario cuando se usa un OpenShift compartido]

Para confirmar si el `containerspec-addcapabilities` está habilitado, puede inspeccionar la configuración actual de `config-features` ejecutando el comando

> ```bash
> oc -n knative-serving get cm config-features -oyaml | grep -c "kubernetes.containerspec-addcapabilities: enabled" && echo "true" || echo "false"
> ```

> **IMPORTANTE** : Si el comando devuelve verdadero, indica que la función 'containerspec-addcapabilities' de Knative ya está habilitada. Omita el paso relacionado con la edición de los permisos de Knative. Sin embargo, si devuelve falso, comuníquese con su instructor al respecto. En un escenario de producción, es posible que deba habilitar manualmente `containerspec-addcapabilities` . Consulte nuestras [instrucciones de configuración de Knative](https://github.com/rhagarty/techxchange-knative-setup) para obtener más información.

### Ejecute los siguientes comandos para proporcionar a las aplicaciones la cuenta de servicio (SA) y la restricción de contexto de seguridad (SCC) correctas para ejecutar instantOn [no es necesario cuando se usa un OpenShift compartido]

```bash
oc create serviceaccount instanton-sa-$CURRENT_NS
oc apply -f scc-cap-cr.yaml
oc adm policy add-scc-to-user cap-cr-scc -z instanton-sa-$CURRENT_NS
```

## 5. Implementar las aplicaciones en OCP

### Actualizar el espacio de nombres en los archivos YAML de implementación

> **NOTA** : Ejecute el siguiente comando para activar el script para actualizar los archivos YAML necesarios con el espacio de nombre del proyecto que configuró previamente (CURRENT_NS)

```bash
./searchReplaceNs.sh
```

### Implementar la aplicación base

> **IMPORTANTE** : asegúrese de completar todos los campos `[Your initial]` con el espacio de nombres utilizado en el paso de creación anterior antes de continuar para aplicar el archivo YAML.

```bash
oc apply -f deploy-without-instanton.yaml
```

### Monitorizar la aplicación base

```bash
oc get pods
```

Una vez que el pod se esté ejecutando y muestre un `POD NAME` , eche un vistazo rápidamente al registro del pod para ver cuánto tiempo tardó la aplicación en iniciarse.

```bash
oc logs <POD NAME>
```

> **NOTA** : Knative detendrá el pod si no recibe una solicitud en el período de tiempo especificado, que se establece en un archivo de configuración yaml. Para este laboratorio, las configuraciones se encuentran en el archivo `serving.yaml` y actualmente están establecidas en 30 segundos (como se muestra a continuación).

```bash
apiVersion: operator.knative.dev/v1beta1
kind: KnativeServing
metadata:
    name: knative-serving
    namespace: knative-serving
spec:
  config:
    autoscaler:
      scale-to-zero-grace-period: "30s"
      scale-to-zero-pod-retention-period: "0s"
```

### Implementar la aplicación con InstantOn

> **IMPORTANTE** : asegúrese de completar todos los campos `[Your initial]` con el espacio de nombres utilizado en el paso de creación anterior antes de continuar para aplicar el archivo YAML.

```bash
oc apply -f deploy-with-instanton.yaml
```

Utilice los mismos comandos `oc get pods` y `oc logs` que se indican anteriormente para supervisar la aplicación.

Compare los tiempos de inicio de ambas aplicaciones y observe cómo la versión InstantOn nuevamente se inicia aproximadamente 10 veces más rápido.

### Verificar las aplicaciones que se ejecutan en el navegador

Para obtener la URL de las aplicaciones implementadas, utilice el siguiente comando:

```bash
oc get ksvc
```

Consulta cada una de las aplicaciones apuntando tu navegador a la URL indicada.

### Pruebe visualmente cuánto tiempo tarda una aplicación inactiva en reiniciarse

Como prueba visual, no haga clic ni actualice ninguna de las aplicaciones que se estén ejecutando en el navegador durante más de 30 segundos. Esto hará que el servicio knative detenga el pod asociado.

Utilice el comando `oc get pods` para verificar que ambos pods de aplicación ya no estén en ejecución.

Ahora, una aplicación a la vez, haga clic en el botón actualizar en la página de la aplicación para ver cuánto tiempo tarda en actualizar el contenido.

> **NOTA** : Knative puede tardar varios segundos en cargarse desde un inicio en frío. Ten esto en cuenta al determinar cuánto tiempo tarda la aplicación en actualizarse.

### (OPCIONAL) Detener y eliminar las aplicaciones implementadas

```bash
oc delete -f deploy-without-instanton.yaml
oc delete -f deploy-with-instanton.yaml
```

## Solución de problemas

### Error al crear la imagen de la aplicación (sin InstantOn)

Si se encuentra con el siguiente error al crear la imagen de la aplicación (sin InstantOn):

```bash
error running container: from /usr/bin/crun creating container for [/bin/sh -c configure.sh]: sd-bus call: Transport endpoint is not connected: Transport endpoint is not connected
: exit status 1
ERRO[0043] did not get container create message from subprocess: EOF
Error: building at STEP "RUN configure.sh": while running runtime: exit status 1
```

Ejecute el siguiente comando desde su ventana de terminal:

```bash
sudo ./build-local-without-instanton.sh
```

### Error al verificar que el operador sin servidor OpenShift esté instalado y listo

Si se encuentra con el siguiente error al ejecutar `oc get csv` :

```bash
oc get csv
No resources found in instantonlab-[your initial] namespace.
```

Espere unos minutos más y vuelva a intentarlo. Debería obtener el resultado correcto.

**===== FIN DEL LABORATORIO =====**

[Regresar a la página principal de laboratorios](../README.md)
