---
lab:
  title: 'Laboratorio 09b: Implementación de Azure Container Instances'
  module: Administer PaaS Compute Options
---

# Laboratorio 09b: Implementación de Azure Container Instances

## Introducción al laboratorio

En este laboratorio, aprenderá a implementar y utilizar Azure Container Instances.

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puedes cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 15 minutos

## Escenario del laboratorio

Tu organización tiene una aplicación web que se ejecuta en una máquina virtual del centro de datos local. La organización quiere mover todas las aplicaciones a la nube, pero no quiere tener un gran número de servidores para administrar. Decide evaluar Azure Container Instances y Docker. 

## Diagrama de arquitectura

![Diagrama de las tareas.](../media/az104-lab09b-aci-architecture.png)

## Aptitudes de trabajo

- Tarea 1: Implementación de Azure Container Instances mediante una imagen de Docker.
- Tarea 2: Pruebe y compruebe la implementación de una instancia de Azure Container Instance.

## Tarea 1: Implementación de Azure Container Instances mediante una imagen de Docker

En esta tarea, creará una aplicación web sencilla mediante una imagen de Docker. Docker es una plataforma que proporciona la capacidad de empaquetar y ejecutar aplicaciones en entornos aislados denominados contenedores. Azure Container Instances proporciona el entorno de proceso para la imagen de contenedor.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. En Azure Portal, busque y seleccione `Container instances` y, después, en la hoja **Instancias de contenedor**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear instancia de contenedor**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | ---- | ---- |
    | Suscripción | Selección de su suscripción a Azure |
    | Resource group | `az104-rg9` (Si es necesario, seleccione **Crear nuevo**) |
    | Nombre del contenedor | `az104-c1` |
    | Region | **Este de EE. UU** (o una región disponible cerca de usted)|
    | Origen de la imagen | **Imágenes de inicio rápido** |
    | Imagen | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Haga clic en **Siguiente: Redes >** y especifique los siguientes ajustes (deje los demás con sus valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Etiqueta de nombre DNS | Cualquier nombre de host DNS válido y único globalmente |

    >**Nota**: el contenedor será accesible públicamente en dns-name-label.region.azurecontainer.io. Si recibes un mensaje de error **La etiqueta de nombre DNS no está disponible**, prueba otro valor.

1. Haz clic en **Siguiente: Supervisión >** y desactiva **Habilitar registros de instancia de contenedor**. 

1. Haz clic en **Siguiente: Avanzado >**, revisa los ajustes sin realizar ningún cambio.

1. Haz clic en **Revisar y crear**, asegúrate de que se ha superado la validación y, a continuación, selecciona **Crear**.

    >**Nota**: espera a que la implementación se complete. Esto debería tardar entre 2 y 3 minutos.

    >**Nota**: mientras esperas, puede que estés interesado en ver el [código que subyace a esta aplicación de ejemplo](https://github.com/Azure-Samples/aci-helloworld). Para ver el código, examina la carpeta \\aplicación.

## Tarea 2: Prueba y comparación de la implementación de una instancia de Azure Container Instance 

En esta tarea, revisarás la implementación de la instancia de contenedor. De forma predeterminada, Azure Container Instance es accesible a través del puerto 80. Una vez implementada la instancia, puedes ir al contenedor mediante el nombre DNS que proporcionaste en la tarea anterior.

1. Cuando finalice la implementación, selecciona el vínculo **Ir al recurso**.

1. En la hoja **Información general** de la instancia de contenedor, comprueba que el **Estado** sea **En ejecución**.

1. Copia el valor de **FQDN** de la instancia de contenedor, abre una nueva pestaña del explorador y ve a la dirección URL correspondiente.

     ![Captura de pantalla de la página de información general de ACI en el portal.](../media/az104-lab09b-aci-overview.png)

1. Comprueba que aparezca la página **Te damos la bienvenida a Azure Container Instances**. Actualiza la página varias veces para crear algunas entradas de registro y, a continuación, cierra la pestaña del explorador.  

1. En la sección **Configuración** de la hoja de instancia del contenedor, haz clic en **Contenedores** y, a continuación, haz clic en **Registros**.

1. Muestra la aplicación en el explorador para comprobar que se ven las entradas de registro que representan la solicitud HTTP GET generada.
   
## Limpieza de los recursos

Si utilizas **tu propia suscripción**, dedica un minuto a eliminar los recursos del laboratorio. De esta forma estarás seguro de que los recursos se liberan y de que se minimiza el coste. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, selecciona el grupo de recursos, selecciona **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haz clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarte a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesitas más información. Abre un explorador Edge y elige Copilot (superior derecha) o ve a *copilot.microsoft.com*. Dedica unos minutos a probar estas indicaciones.

+ Resumir los pasos para crear y configurar una instancia de contenedor Azure.
+ ¿Cuáles son las formas en que puedo ejecutar un contenedor sin servidor en Azure?

## Más información con el aprendizaje autodirigido

+ [Ejecución de imágenes de contenedor en Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/). Descubre cómo Azure Container Instances puede ayudarle a implementar rápidamente contenedores, a establecer variables de entorno y a especificar directivas de reinicio de contenedores.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Azure Container Instances (ACI) es un servicio que permite implementar contenedores en la nube pública de Microsoft Azure.
+ ACI no requiere que aprovisiones ni administres ninguna infraestructura subyacente.
+ ACI admite contenedores de Linux y contenedores de Windows.
+ Las cargas de trabajo en ACI suelen iniciarse y detenerse por algún tipo de proceso o desencadenador y suelen ser de corta duración. 

    
