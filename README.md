# 🚀 **Trabajo 0311AT - Despliegue de Página Web Estática en Minikube**

Este proyecto tiene como objetivo desplegar una **página web estática** en Minikube, utilizando **Kubernetes** y un **volumen persistente**. La página está vinculada a un **repositorio GitHub** para mantener el contenido actualizado. Se utiliza **Nginx** como servidor web para servir los archivos estáticos.

---

## 🔧 **Requisitos**

Para llevar a cabo este proyecto, necesitarás las siguientes herramientas:

- **Minikube**: Para crear y administrar el clúster de Kubernetes local. Si no lo tienes, sigue la [guía de instalación oficial](https://minikube.sigs.k8s.io/docs/).
- **Kubectl**: Herramienta de línea de comandos para interactuar con el clúster de Kubernetes. Puedes instalarla siguiendo la [guía oficial](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
- **Docker**: Si vas a usar el driver Docker para Minikube, asegúrate de tener Docker instalado en tu máquina.

---
## 📝 **Pasos para Desplegar el Entorno**

## 1️⃣ **Clonar los Repositorios**

Primero, necesitamos clonar dos repositorios: uno que contiene el contenido de la página web estática y otro con los manifiestos de Kubernetes. Para ello, ejecuta los siguientes comandos:

```bash
git clone https://github.com/gabimac03/static-website.git
git clone https://github.com/gabimac03/manifestk8s.git
```
Tus carpetas se deben ver algo así, podemos organizarlas más todavía si queremos
```
-----------------------------
/desktop/tu-usuario
│   ├── manifestk8s
│        └── deploy.yaml
│        └── ingress.yaml
│        └── service.yaml
│        └── pvc.yaml
│        └── pv.yaml
└── static-website
    ├── index.html
    └── style.css
```

## 2️⃣ Despliegue de Página Web Estática en Minikube

Este proyecto tiene como objetivo desplegar una página web estática utilizando Kubernetes en Minikube. A continuación se describe el proceso para configurar y montar el directorio que contiene el contenido estático de la página web en el entorno de Minikube.

### Iniciar Minikube con Montaje de Directorio

1. **Iniciar Minikube**: Para comenzar, necesitas iniciar Minikube utilizando el driver Docker y montar la carpeta local que contiene el contenido estático de la página web.

   Sustituye `admin` por el nombre de la carpeta correspondiente en tu entorno local. Asegúrate de que la ruta del directorio sea correcta.

   Ejecuta el siguiente comando para iniciar Minikube:

   ```bash
   minikube start --driver=docker --mount --mount-string="/Admin/Desktop/static-website:/mnt/web"

   ```
## 3️⃣ Aplicar los Manifiestos de Kubernetes

Una vez que Minikube esté corriendo 🏃‍♂️💻, el siguiente paso es aplicar los archivos YAML que contienen la configuración de los recursos de Kubernetes. Estos archivos definen los siguientes elementos:

- **PV** (Persistent Volume) 📦
- **PVC** (Persistent Volume Claim) 🔒
- **Deployment** 🚀
- **Service** 🌐
- **Ingress** 🔑

Sigue estos pasos:

1. **Aplicar los Manifiestos**: Ejecuta los siguientes comandos en tu terminal para aplicar cada uno de los manifiestos YAML.

   ```bash
   cd /Admin/Desktop/manifestk8s/  # Cambia al directorio de los manifiestos

   cd
   
   kubectl apply -f /Admin/Desktop/manifestk8s/pv.yaml
   kubectl apply -f /Admin/Desktop/manifestk8s/pvc.yaml
   kubectl apply -f /Admin/Desktop/manifestk8s/deploy.yaml
   kubectl apply -f /Admin/Desktop/manifestk8s/service.yaml
   kubectl apply -f /Admin/Desktop/manifestk8s/ingress.yaml

   ```
## 4️⃣ Verificar los Recursos Desplegados

Para asegurarte de que todo esté funcionando correctamente ✅, es importante verificar el estado de los pods y los servicios en Kubernetes. Sigue estos pasos:

1. **Verificar el Estado de los Pods**: Ejecuta el siguiente comando para comprobar que los pods estén en estado `Running`:

   ```bash
   kubectl get pods  # Verifica que los pods estén en estado Running ✅
   ```
## 5️⃣ Habilitar Ingress

Si aún no has habilitado el addon de **Ingress** en Minikube, sigue estos pasos para habilitarlo y verificar su estado:

1. **Habilitar el Addon de Ingress**: Ejecuta el siguiente comando para habilitar el addon de Ingress en Minikube:

   ```bash
    minikube addons enable ingress  # Habilita el addon de Ingress 🔑
2. Verificar el Estado de Ingress: Una vez habilitado el addon, puedes verificar su estado con el siguiente comando:

   ```bash
    kubectl get ingress  # Verifica el estado de Ingress 🔑

## 6️⃣ Configurar el Archivo /etc/hosts

Una vez que **Ingress** esté configurado y los servicios estén corriendo ✅, vamos a añadir una entrada en el archivo `/etc/hosts` para poder acceder a la página web usando un nombre de dominio personalizado, como `sitio.local`.

1. **Añadir Entrada en el Archivo `/etc/hosts`**: Ejecuta el siguiente comando para vincular la IP de Minikube al dominio `sitio.local`:

   ```bash
   echo "$(minikube ip) sitio.local" | sudo tee -a /etc/hosts  # Vincula la IP de Minikube al dominio sitio.local 🌐
   Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "127.0.0.1 sitio.local" # Windows
2. Iniciar el minikube tunnel
Para poder redirigir el tráfico de tu dominio personalizado a Minikube, debes iniciar el túnel de Minikube. Ejecuta el siguiente comando para crear el túnel:

```bash
minikube tunnel  # Crea un túnel entre Minikube y tu red local 🌐
Este comando abrirá un túnel en el que Minikube gestionará el tráfico hacia tu clúster de Kubernetes.
```
Este comando hace lo siguiente:

 1. Obtiene la IP de Minikube con minikube ip.
 2. Añade una entrada en el archivo /etc/hosts, asociando esa IP al nombre de dominio sitio.local.
Acceder a la Página Web: Ahora, puedes acceder a tu aplicación desde el navegador usando el nombre de dominio sitio.local en lugar de la IP directa de Minikube. Solo abre tu navegador y visita:
http://sitio.local

## 📌 Notas Importantes
  1. Asegúrate de tener Minikube y Docker corriendo antes de iniciar el proyecto.
  2. No olvides ejecutar minikube tunnel si estás usando Ingress, ya que es necesario para enrutar el tráfico a tu servicio.
  3. Si deseas cambiar el nombre del dominio de sitio.local a otro, simplemente modifica la entrada en el archivo /etc/hosts y debes modificar el ingress.

# ¡Feliz Desarrollo! 🎉
