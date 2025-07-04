---
lab:
  title: 'Laboratorio 09c: Implementación de Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Laboratorio 09c: Implementación de Azure Container Apps

## Introducción al laboratorio

En este laboratorio, aprenderás a implementar Azure Container Apps.

Este laboratorio requiere una suscripción a Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puedes cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 15 minutos

## Escenario del laboratorio

Tu organización tiene una aplicación web que se ejecuta en una máquina virtual del centro de datos local. La organización quiere mover todas las aplicaciones a la nube sin tener que administrar muchos servidores. Por consiguiente, decides evaluar Azure Container Apps.

## Diagrama de arquitectura

![Diagrama de las tareas.](../media/az104-lab09b-aca-architecture.png)

## Aptitudes de trabajo

- Tarea 1: Creación y configuración de una instancia de Azure Container Apps y de su entorno.
- Tarea 2: Prueba y comprobación de la implementación de la instancia de Azure Container Apps.

## Tarea 1: Creación y configuración de una instancia de Azure Container Apps y de su entorno

Azure Container Apps lleva el concepto de clúster de Kubernetes administrado un paso más allá, ya que no solo administra el entorno del clúster, sino que también proporciona otros servicios administrados, además del clúster. A diferencia de los clústeres de Azure Kubernetes, en los que es preciso administrar el clúster, las instancias de Azure Container Apps eliminan una parte de la complejidad de configurar los clústeres de Kubernetes.

1. En Azure Portal, busca `Container Apps` y selecciónalo.

1. Selecciona **+ Crear**, en el menú desplegable **Aplicación web**. Observa las otras opciones. 

1. Usa la siguiente información para rellenar los detalles de la pestaña **Datos básicos**.*.

    | Configuración | Acción |
    |---|---|
    | Suscripción | Selecciona tu suscripción a Azure. |
    | Grupo de recursos | `az104-rg9` |
    | Nombre de la aplicación de contenedor |  `my-app` |
    | Región    | **Este de EE. UU.** (|
    | Entorno de Container Apps | Selecciona **Crear nuevo** > Establecer nombre de entorno para `my-environment` > **Crear** |

1. Haz clic en la pestaña **Siguiente: Contenedor** y asegúrate de que **Usar imagen de inicio rápido** esté activada. Es posible que tengas que desplazarte hacia arriba para ver esta configuración. 

1. Deja **Imagen de inicio rápido** en **contenedor sencillo Hola mundo**. Observa las otras opciones. 

1. Selecciona **Revisar y crear** y, después, **Crear**.

    >**Nota:** espera hasta que se implemente la aplicación de contenedor. Este proceso tardará unos minutos. 
 
## Tarea 2: Prueba y comprobación de la implementación de la instancia de Azure Container Apps.

De forma predeterminada, la instancia de Azure Container Apps que crees va a aceptar el tráfico en el puerto 80 mediante la aplicación de ejemplo Hola mundo. Azure Container Apps proporcionará un nombre DNS a la aplicación. Copia esta dirección URL y ve a ella para asegurarte de que la aplicación está en funcionamiento.

1. Selecciona **Ir al recurso** para ver la aplicación de contenedor.

1. Selecciona el vínculo que hay junto a *URL de aplicación* para ver la aplicación.

    ![Captura de pantalla de la página de información general de ACA en el portal.](../media/az104-lab09b-aca-overview.png)

1. Compruebe que recibe el mensaje **La aplicación de Azure Container Apps está activa**.
   
## Limpieza de los recursos

Si utilizas **tu propia suscripción**, dedica un minuto a eliminar los recursos del laboratorio. De esta forma estarás seguro de que los recursos se liberan y de que se minimiza el coste. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, selecciona el grupo de recursos, selecciona **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haz clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarte a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesitas más información. Abre un explorador Edge y elige Copilot (superior derecha) o ve a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ Resumir los pasos para crear y configurar una aplicación de contenedor de Azure.
+ Compare y contraste Azure Container Apps con Azure Kubernetes Service.

## Más información con el aprendizaje autodirigido

+ [Configure aplicaciones contenedoras en Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/). Examina las características y funcionalidades de Azure Container Apps y, después, se centra en cómo crear, configurar, escalar y administrar aplicaciones contenedoras mediante Azure Container Apps.


## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones del laboratorio. 

+ Azure Container Apps (ACA) es una plataforma sin servidor que permite mantener menos infraestructura y ahorrar costos al ejecutar aplicaciones contenedorizadas.
+ Container Apps proporciona configuración del servidor, orquestación de contenedores y detalles de la implementación. 
+ En Azure Container Apps, las cargas de trabajo suelen ser procesos de ejecución prolongada como una aplicación web.

     
