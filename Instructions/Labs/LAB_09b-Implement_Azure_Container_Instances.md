---
lab:
  title: "Laboratorio\_09b: Implementación de Azure Container Instances"
  module: Administer PaaS Compute Options
---

# Laboratorio 09b: Implementación de Azure Container Instances

## Introducción al laboratorio

En este laboratorio, aprenderá a implementar y utilizar Azure Container Instances.

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 15 minutos

## Escenario del laboratorio

Su organización tiene una aplicación web que se ejecuta en una máquina virtual del centro de datos local. La organización quiere mover todas las aplicaciones a la nube, pero no quiere tener un gran número de servidores para administrar. Decide evaluar Azure Container Instances y Docker. 
## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se requiere una suscripción de Azure.

+ [Implementación de Azure Container Instances](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203). Cree, configure e implemente un contenedor de Docker con Azure Container Instances.
  
+ [Implementar Azure Container Instances](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014).  Implementación de una imagen de Docker por medio de Azure Container Instances. 

## Diagrama de la arquitectura

![Diagrama de las tareas.](../media/az104-lab09b-aci-architecture.png)

## Aptitudes de trabajo

- Tarea 1: Implementación de Azure Container Instances mediante una imagen de Docker.
- Tarea 2: Pruebe y compruebe la implementación de una instancia de Azure Container Instance.

## Tarea 1: Implementación de Azure Container Instances mediante una imagen de Docker

En esta tarea, creará una aplicación web sencilla mediante una imagen de Docker. Docker es una plataforma que proporciona la capacidad de empaquetar y ejecutar aplicaciones en entornos aislados denominados contenedores. Azure Container Instances proporciona el entorno de proceso para la imagen de contenedor.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. En Azure Portal, busque y seleccione `Container instances` y, después, en la hoja **Instancias de contenedor**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear instancia de contenedor**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | ---- | ---- |
    | Suscripción | Seleccione su suscripción a Azure. |
    | Resource group | `az104-rg9` (Si es necesario, seleccione **Crear nuevo**) |
    | Nombre del contenedor | `az104-c1` |
    | Region | **Este de EE. UU** (o una región disponible cerca de usted)|
    | Origen de la imagen | **Imágenes de inicio rápido** |
    | Imagen | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Haga clic en **Siguiente: Redes >** y especifique los siguientes ajustes (deje los demás con sus valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Etiqueta de nombre DNS | Cualquier nombre de host DNS válido y único globalmente |

    >**Nota**: El contenedor será accesible públicamente en dns-name-label.region.azurecontainer.io. Si recibe un mensaje de error **La etiqueta de nombre DNS no está disponible**, pruebe otro valor.

1. Haga clic en **Siguiente: Avanzado >**, revise los ajustes sin realizar ningún cambio.

 1. Haga clic en **Revisar y crear**, asegúrese de que se ha superado la validación y, a continuación, seleccione **Crear**.

    >**Nota**: Espere a que la implementación se complete. Esto debería tardar entre 2 y 3 minutos.

    >**Nota**: Mientras espera, puede que esté interesado en ver el [código que subyace a esta aplicación de ejemplo](https://github.com/Azure-Samples/aci-helloworld). Para ver el código, examine la carpeta \\aplicación.

## Tarea 2: Pruebe y compruebe la implementación de una instancia de Azure Container Instance 

En esta tarea, revise la implementación de la instancia de contenedor. De forma predeterminada, Azure Container Instance es accesible a través del puerto 80. Una vez implementada la instancia, puede ir al contenedor mediante el nombre DNS que proporcionó en la tarea anterior.

1. En la hoja de implementación, seleccione el vínculo **Ir al recurso**.

1. En la hoja **Información general** de la instancia de contenedor, compruebe que el **Estado** sea **En ejecución**.

1. Copie el valor de **FQDN** de la instancia de contenedor, abra una nueva pestaña del explorador y vaya a la dirección URL correspondiente.

     ![Captura de pantalla de la página de información general de ACI en el portal.](../media/az104-lab09b-aci-overview.png)

1. Compruebe que aparezca la página **Le damos la bienvenida a Azure Container Instances**. Actualice la página varias veces para crear algunas entradas de registro y, a continuación, cierre la pestaña del explorador.  

1. En la sección **Configuración** de la hoja de instancia del contenedor, haga clic en **Contenedores** y, a continuación, haga clic en **Registros**.

1. Muestre la aplicación en el explorador para comprobar que se ven las entradas de registro que representan la solicitud HTTP GET generada.
   
## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ Resumir los pasos para crear y configurar una instancia de contenedor Azure.
+ ¿Cuáles son las formas en que puedo ejecutar un contenedor sin servidor en Azure?

## Más información con el aprendizaje autodirigido

+ [Ejecución de imágenes de contenedor en Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/). Descubra cómo Azure Container Instances puede ayudarle a implementar rápidamente contenedores, a establecer variables de entorno y a especificar directivas de reinicio de contenedores.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Azure Container Instances (ACI) es un servicio que permite implementar contenedores en la nube pública de Microsoft Azure.
+ ACI no requiere que aprovisione ni administre ninguna infraestructura subyacente.
+ ACI admite contenedores de Linux y contenedores de Windows.
+ Las cargas de trabajo en ACI suelen iniciarse y detenerse por algún tipo de proceso o desencadenador y suelen ser de corta duración. 

    
