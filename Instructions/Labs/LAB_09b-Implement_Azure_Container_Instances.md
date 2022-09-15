---
lab:
  title: '09b: Implementación de Azure Container Instances'
  module: Administer Serverless Computing
---

# <a name="lab-09b---implement-azure-container-instances"></a>Laboratorio 09b: Implementación de Azure Container Instances
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

Contoso wants to find a new platform for its virtualized workloads. You identified a number of container images that can be leveraged to accomplish this objective. Since you want to minimize container management, you plan to evaluate the use of Azure Container Instances for deployment of Docker images.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

- Tarea 1: Implementar una imagen de Docker mediante Azure Container Instances
- Tarea 2: Revisar la funcionalidad de Azure Container Instances

## <a name="estimated-timing-20-minutes"></a>Tiempo estimado: 20 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab09b.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-deploy-a-docker-image-by-using-the-azure-container-instance"></a>Tarea 1: Implementar una imagen de Docker mediante Azure Container Instances

En esta tarea, creará una nueva instancia de contenedor para la aplicación web.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En Azure Portal, busque **Instancias de contenedor** y luego, en la hoja **Instancias de contenedor**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear instancia de contenedor**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | ---- | ---- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Resource group | Nombre de un nuevo grupo de recursos **az104-09b-rg1** |
    | Nombre del contenedor | **az104-9b-c1** |
    | Region | Nombre de una región donde puede aprovisionar Azure Container Instances |
    | Origen de la imagen | **Imágenes de inicio rápido** |
    | Imagen | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Haga clic en **Siguiente: Redes >** y, en la pestaña **Redes** de la hoja **Crear instancia de contenedor**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Etiqueta de nombre DNS | Cualquier nombre de host DNS válido y único globalmente |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Your container will be publicly reachable at dns-name-label.region.azurecontainer.io. If you receive a <bpt id="p1">**</bpt>DNS name label not available<ept id="p1">**</ept> error message, specify a different value.

1. Haga clic en **Siguiente: Opciones avanzadas >** , revise la configuración en la pestaña **Opciones avanzadas** de la hoja **Crear instancia de contenedor** sin realizar ningún cambio, haga clic en **Revisar y crear**, asegúrese de que se supere la validación y haga clic en **Crear**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 3 minutes.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While you wait, you may be interested in viewing the <bpt id="p2">[</bpt>code behind the sample application<ept id="p2">](https://github.com/Azure-Samples/aci-helloworld)</ept>. To view it, browse the <ph id="ph1">\\</ph>app folder.

#### <a name="task-2-review-the-functionality-of-the-azure-container-instance"></a>Tarea 2: Revisar la funcionalidad de Azure Container Instances

En esta tarea, revisará la implementación de la instancia de contenedor.

1. En la hoja de implementación, seleccione el vínculo **Ir al recurso**.

1. En la hoja **Información general** de la instancia de contenedor, compruebe que el **Estado** sea **En ejecución**.

1. Copie el valor de **FQDN** de la instancia de contenedor, abra una nueva pestaña del explorador y vaya a la dirección URL correspondiente.

1. Compruebe que aparezca la página **Le damos la bienvenida a Azure Container Instances**.

1. Cierre la nueva pestaña del explorador, de nuevo en Azure Portal, en la sección **Configuración** de la hoja de la instancia de contenedor, haga clic en **Contenedores** y, a continuación, haga clic en **Registros**.

1. Muestre la aplicación en el explorador para comprobar que se ven las entradas de registro que representan la solicitud HTTP GET generada.

#### <a name="clean-up-resources"></a>Limpieza de recursos

>Contoso quiere encontrar una nueva plataforma para sus cargas de trabajo virtualizadas.

>Se han detectado varias imágenes de contenedor que se pueden aprovechar para lograr este objetivo. 

1. En Azure Portal, abra la sesión de **PowerShell** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro -AsJob). Aunque podrá ejecutar otro comando de PowerShell inmediatamente después en la misma sesión de PowerShell, los grupos de recursos tardarán unos minutos en eliminarse.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Implementado una imagen de Docker mediante Azure Container Instances
- Revisado la funcionalidad de Azure Container Instances
