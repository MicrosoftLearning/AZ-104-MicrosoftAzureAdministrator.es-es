---
lab:
  title: "Laboratorio\_09c: Implementación de Azure Container Apps"
  module: Administer PaaS Compute Options
---

# Laboratorio 09c: Implementación de Azure Container Apps

## Introducción al laboratorio

En este laboratorio, aprenderá a implementar Azure Container Apps.

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 15 minutos

## Escenario del laboratorio

Su organización tiene una aplicación web que se ejecuta en una máquina virtual del centro de datos local. La organización quiere mover todas las aplicaciones a la nube sin tener que administrar muchos servidores. Por consiguiente, decide evaluar Azure Container Apps.

## Simulaciones de laboratorio interactivas

No hay simulaciones de laboratorio interactivas para este tema. 

## Aptitudes de trabajo

- Tarea 1: Cree y configure una instancia de Azure Container Apps y de su entorno.
- Tarea 2: Pruebe y compruebe la implementación de la instancia de Azure Container Apps.

## Diagrama de la arquitectura

![Diagrama de las tareas.](../media/az104-lab09b-aca-architecture.png)

## Tarea 1: Cree y configure una instancia de Azure Container Apps y de su entorno

Azure Container Apps lleva el concepto de clúster de Kubernetes administrado un paso más allá, ya que no solo administra el entorno del clúster, sino que también proporciona otros servicios administrados, además del clúster. A diferencia de los clústeres de Azure Kubernetes, en los que es preciso administrar el clúster, las instancias de Azure Container Apps eliminan una parte de la complejidad de configurar los clústeres de Kubernetes.

1. En Azure Portal, busque `Container Apps` y selecciónelo.

1. En **Container Apps**, seleccione **Crear**.

1. Use la siguiente información para rellenar los detalles de la pestaña **Aspectos básicos**.*.

    | Configuración | Acción |
    |---|---|
    | Subscription | Seleccione su suscripción a Azure. |
    | Resource group | `az104-rg9` |
    | Nombre de la aplicación de contenedor |  `my-app` |
    | Region    | **Este de EE. UU** (o cualquier región disponible que tenga cerca) |
    | Entorno de Container Apps | Deje el valor predeterminado |

1. En la pestaña **Contenedor**, asegúrese de que la opción **Usar imagen de inicio rápido** está habilitada y de que la imagen del inicio rápido está establecida en **Contenedor Hola mundo simple**.

1. Seleccione **Revisar y crear** y, después, **Crear**.

    >**Nota:** Espere hasta que se implemente la aplicación contenedora. Este proceso tardará unos minutos. 
 
## Tarea 2: Pruebe y compruebe la implementación de la instancia de Azure Container Apps.

De forma predeterminada, la instancia de Azure Container Apps que cree va a aceptar el tráfico en el puerto 80 mediante la aplicación de ejemplo Hola mundo. Azure Container Apps proporcionará un nombre DNS a la aplicación. Copie esta dirección URL y vaya a ella para asegurarse de que la aplicación está en funcionamiento.

1. Seleccione **Ir al recurso** para ver la aplicación de contenedor.

1. Seleccione el vínculo que hay junto a *URL de aplicación* para ver la aplicación.

    ![Captura de pantalla de la página de información general de ACA en el portal.](../media/az104-lab09b-aca-overview.png)

1. Compruebe que recibe el mensaje **La aplicación de Azure Container Apps está activa**.
   
## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. Así tendrá la certeza de que los recursos están libres y de que se minimiza el coste. La manera más fácil de eliminar los recursos de laboratorio consiste en eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escriba el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.



## Puntos clave

Enhorabuena, ha completado el laboratorio. Estas son las principales conclusiones del laboratorio. 

+ Azure Container Apps (ACA) es una plataforma sin servidor que permite mantener menos infraestructura y ahorrar costos al ejecutar aplicaciones contenedorizadas.
+ Container Apps proporciona configuración del servidor, orquestación de contenedores y detalles de la implementación. 
+ En Azure Container Apps, las cargas de trabajo suelen ser procesos de ejecución prolongada como una aplicación web.

## Más información con el aprendizaje autodirigido

+ [Configure aplicaciones contenedoras en Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/). Examina las características y funcionalidades de Azure Container Apps y, después, se centra en cómo crear, configurar, escalar y administrar aplicaciones contenedoras mediante Azure Container Apps.
     
