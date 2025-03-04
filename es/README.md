# Introducción a los laboratorios

Siga esta guía para completar los laboratorios prácticos para Liberty

Para poder completar todos los laboratorios, se han previsto diferentes entornos. Cada entorno es independiente de los demás, por lo que depende de las habilidades y el interés, pero sugerimos el siguiente orden:

## 1. Introducción a Liberty

Descubra los conceptos básicos de WebSphere Liberty y cómo comenzar a desarrollar en él.

**Entorno de taller Liberty - VMWare**

URL de VNC: proporcionada por el instructor

**Laboratorios:**

- [101-Liberty-Getting-Started](101-Liberty-Getting-Started/README.md)
- [102-Discovery-Liberty](102-Discovery-Liberty/README.md)
- [103-Liberty-devmode-VSCode](103-Liberty-devmode-VSCode/README.md)

## 2. Liberty Collectives

Descubra la arquitectura colectiva para Liberty y cómo proporcionar equilibrio de carga dinámico y alta disponibilidad. Por último, experimente la arquitectura de migración cero.

**Implementación empresarial en el entorno Liberty**

URL de VNC: proporcionada por el instructor

**Laboratorios:**

- [111-Liberty-Collectives](111-Liberty-Collectives/README.md)
- [112-Liberty-Dynamic-Routing](112-Liberty-Dynamic-Routing/README.md)
- [113-Liberty-Zero-Migration](113-Liberty-Zero-Migration/README.md)

## 3. Liberty en contenedores

Descubra cómo trasladar Liberty de una máquina virtual a un entorno basado en contenedores.

Estos laboratorios le presentarán primero los contenedores y luego OpenShift, para finalmente guiarlo sobre cómo implementar Liberty en un clúster OpenShift.

**131-Intro-Containers** y **132-Intro-OpenShift** son opcionales si ya está familiarizado con el uso de podman cli para administrar contenedores y usar OpenShift.

**133-Liberty-on-OpenShift** Implementación de contenedores Liberty con CP4Apps en OpenShift

URL de VNC: proporcionada por el instructor

**Laboratorios:**

- [131-Intro-Containers](131-Intro-Containers/README.md)
- [132-Intro-OpenShift](132-Intro-OpenShift/README.md)
- [133-Liberty-on-OpenShift](133-Liberty-on-OpenShift/README.md)

## 4. Encendido instantáneo de Liberty

Descubra la capacidad de InstantOn para habilitar su aplicación Liberty para arquitecturas sin servidor y casos de uso impulsados por eventos.

Este laboratorio se basará en dos entornos diferentes: el del laboratorio anterior y un clúster OpenShift compartido debido a las dependencias de versiones.

**Implementación de contenedores Liberty con CP4Apps en OpenShift**

URL de VNC: proporcionada por el instructor

**Clúster OpenShift compartido:** 
https://console-openshift-console.apps.67a3346aadf42b5f8ef28cdc.eu1.techzone.ibm.com/

nombre de usuario: usuario[x] contraseña: passw0rd

**Laboratorios:**

- [141-Liberty-InstantOn-Serverless](141-Liberty-InstantOn-Serverless/README.md)
