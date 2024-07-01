---
lab:
  title: "Laboratorio\_03: Administración de recursos de Azure mediante plantillas de Azure Resource Manager"
  module: Administer Azure Resources
---

# Laboratorio 03: Administración de recursos de Azure mediante plantillas de Azure Resource Manager

## Introducción al laboratorio

En este laboratorio, aprenderá a automatizar las implementaciones de recursos. Obtenga información sobre las plantillas de Azure Resource Manager y las plantillas de Bicep. Obtendrá información sobre las distintas formas de implementar las plantillas. 

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.** 

## Tiempo estimado: 50 minutos

## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se requiere una suscripción de Azure. 

+ [Administración de recursos de Azure mediante plantillas de Azure Resource Manager](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205) Revise, cree e implemente discos administrados con una plantilla.
  
+ [Creación de una máquina virtual con una plantilla](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Implementación desde una máquina virtual con una plantilla de inicio rápido.
  
## Escenario del laboratorio

Su equipo quiere ver formas de automatizar y simplificar las implementaciones de recursos. Su organización busca formas de reducir la sobrecarga administrativa, reducir el error humano y aumentar la coherencia.  

## Diagrama de la arquitectura

![Diagrama de las tareas.](../media/az104-lab03-architecture.png)

## Aptitudes de trabajo

+ Tarea 1: Cree una plantilla de Azure Resource Manager.
+ Tarea 2: Edite una plantilla de Azure Resource Manager y vuelva a implementar la plantilla.
+ Tarea 3: Configuración de Cloud Shell e implementación de una plantilla con Azure PowerShell.
+ Tarea 4: Implementación de una plantilla con la CLI. 
+ Tarea 5: Implementación de un recurso mediante Azure Bicep.

## Tarea 1: crear una Azure Resource Manager plantilla

En esta tarea, crearemos un disco administrado en Azure Portal. Los discos administrados están diseñados para usarse con máquinas virtuales. Una vez implementado el disco, exportará una plantilla que puede usar en otras implementaciones.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `Disks`.

1. En la página Discos, seleccione **Crear**.

1. En la página **Crear un disco administrado**, configure el disco y seleccione **Aceptar**. 
    
    | Configuración | Valor |
    | --- | --- |
    | Suscripción | *su suscripción* | 
    | Grupo de recursos | `az104-rg3` (Si es necesario, seleccione **Crear nuevo**).
    | Nombre del disco | `az104-disk1` | 
    | Region | **Este de EE. UU.** |
    | Zona de disponibilidad | **No se requiere redundancia de la infraestructura** | 
    | Tipo de origen | **Ninguno** |
    | Rendimiento | **HDD estándar** (cambiar tamaño) |
    | Size | **32 GiB** | 

    >**Nota:** Estamos creando un disco administrado sencillo para que pueda practicar con plantillas. Los discos administrados de Azure son volúmenes de almacenamiento de nivel de bloque que Azure administra.

1. Haga clic en **Revisar y crear** y seleccione **Crear**.

1. Supervise las notificaciones (superior derecha) y, después de la implementación, seleccione **Ir al recurso**. 

1. En la hoja **Automatización**, seleccione **Exportar plantilla**. 

1. Tómese un minuto para revisar los archivos **Plantilla** y **Parámetros**.

1. Haga clic en **Descargar** y guarde las plantillas en la unidad local. Esto crea un archivo comprimido. 

1. Utilice el Explorador de archivos para extraer el contenido del archivo descargado en la carpeta **Descargas** de su ordenador. Observe que hay dos archivos JSON (plantilla y parámetros). 

   >**¿Sabía que...?**  Puede exportar todo un grupo de recursos o solo recursos específicos dentro de ese grupo de recursos.

## Tarea 2: Edite una plantilla de Azure Resource Manager y, a continuación, vuelva a implementar la plantilla.

En esta tarea, usará la plantilla descargada para implementar un nuevo disco administrado. En esta tarea se describe cómo repetir las implementaciones de forma rápida y sencilla. 

1. En Azure Portal, busque y seleccione `Deploy a custom template`.

1. En la hoja **Implementación personalizada**, observe que existe la posibilidad de utilizar una **plantilla de inicio rápido**. Hay muchas plantillas integradas, como se muestra en el menú desplegable. 

1. En lugar de usar un inicio rápido, seleccione **Compilar su propia plantilla en el editor**.

1. En la hoja **Editar plantilla**, haga clic en **Cargar archivo** y cargue el archivo **template.json** que descargó en la tarea anterior.

1. En el panel del editor, realice estos cambios.

    + Cambie **disks_az104_disk1_name** a `disk_name` (dos lugares para cambiar)
    + Cambie **az104-disk1** a `az104-disk2` (un lugar para cambiar)

1. Observe que se trata de un disco **estándar**. La ubicación es **eastus**. El tamaño del disco es de **32 GB**.

1. Guarde los cambios mediante **Guardar**.

1. No olvide el archivo de parámetros. Seleccione **Editar parámetros**, haga clic en **Cargar archivo** y cargue el **parameters.json**. 

1. Realice este cambio para que coincida con el archivo de plantilla.

    Cambie **disks_az104_disk1_name** a **disk_name** (un lugar para cambiar)

1. Guarde los cambios mediante **Guardar**. 

1. Complete la configuración de implementación personalizada:

    | Configuración | Valor |
    | --- |--- |
    | Suscripción | *su suscripción* |
    | Grupo de recursos | `az104-rg3` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Nombre del disco | `az104-disk2` |

1. Seleccione **Revisar y crear** y, luego, **Crear**.

1. Haga clic en **Go to resource** (Ir al recurso). Compruebe que se creó **az104-disk2**.

1. En la hoja **Información general**, seleccione el grupo de recursos **az104-rg3**. Ahora debería tener dos discos.
   
1. En la sección **Configuración**, haga clic en **Implementaciones**.

    >**Nota:** Todos los detalles de las implementaciones se documentan en el grupo de recursos. Se recomienda revisar las primeras implementaciones basadas en plantillas para garantizar el éxito antes de usar las plantillas para las operaciones a gran escala.

1. Seleccione una implementación y revise el contenido de las hojas **Entrada** y **Plantilla**.

## Tarea 3: Configuración de Cloud Shell e implementación de una plantilla con PowerShell 

En esta tarea, trabajará con Azure Cloud Shell y Azure PowerShell. Azure Cloud Shell es un terminal interactivo, autenticado y al que se puede acceder desde un explorador para administrar recursos de Azure. Ofrece la flexibilidad de poder elegir la experiencia de shell que mejor se adapte a la forma de trabajar de cada uno, Bash o PowerShell. En esta tarea, usará PowerShell para implementar una plantilla. 

1. Seleccione el icono **Cloud Shell** en la parte superior derecha de Azure Portal. Como alternativa, puede navegar directamente a `https://shell.azure.com`.

   ![Captura de pantalla del icono de Cloud Shell.](../media/az104-lab03-cloudshell-icon.png)

1. Cuando se le pida que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**. 

    >**¿Sabía que...?**  Si trabaja principalmente con sistemas Linux, Bash (CLI) le resultará más familiar. Si trabaja principalmente con sistemas Windows, Azure PowerShell le resultará más familiar. 

1. En la pantalla **Introducción**, seleccione **Montar una cuenta de almacenamiento**, seleccione la **suscripción de la cuenta de almacenamiento** y, a continuación, seleccione **Aplicar**.

1. Seleccione **Quiero crear una cuenta de almacenamiento** y, a continuación, **Siguiente**. Complete la información de **Crear cuenta de almacenamiento**. 
    
    | Configuración | Valores |
    |  -- | -- |
    | Grupo de recursos | **az104-rg3** |
    | Region | *seleccione la región* | 
    | Cuenta de almacenamiento (Crear nueva) | * debe ser único globalmente, entre 3 y 24 caracteres de longitud y usar números y solo letras minúsculas* |
    | Recurso compartido de archivos (Crear nuevo) | `fs-cloudshell` |

1. Cuando haya terminado, seleccione **Crear**.

    >Tardará un par de minutos en aprovisionar el almacenamiento.

1. Seleccione **Configuración** (barra superior) y, a continuación, **Ir a la versión clásica**.

1. Seleccione el icono **Cargar o descargar archivos** (barra superior) y, a continuación, seleccione **Cargar**.

1. Cargue los archivos de plantilla y parámetros desde el directorio **Descargas**. 

1. Seleccione el icono **Editor (corchetes)** y vaya hasta el archivo JSON de plantilla de la izquierda en el panel de navegación.

1. Realizar un cambio. Por ejemplo, cambie el nombre del disco a **az104-disk3**. Presione **Ctrl+S** para guardar los cambios. 

    >**Nota**: La implementación de la plantilla puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. Según el ámbito de la implementación, usará comandos diferentes.

1. Para realizar la implementación en un grupo de recursos, utilice **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. Asegúrese de que el comando se completa y ProvisioningState es **Correcto**.

1. Confirme que se creó el disco.

   ```powershell
   Get-AzDisk
   ```
   
## Tarea 4: Implementación de una plantilla con la CLI 

1. Continúe en **Cloud Shell**, seleccione **Bash**. **Confirme** la selección.

1. Compruebe que los archivos están disponibles en el almacenamiento de Cloud Shell. Si completó la tarea anterior, los archivos de plantilla deben estar disponibles. 

    ```sh
    ls
    ```

1. Seleccione el icono **Editor** (corchetes) y vaya hasta el archivo JSON de plantilla.

1. Realizar un cambio. Por ejemplo, cambie el nombre del disco a **az104-disk4**. Presione **Ctrl+S** para guardar los cambios. 

    >**Nota**: La implementación de la plantilla puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. Según el ámbito de la implementación, usará comandos diferentes.

1. Para realizar la implementación en un grupo de recursos, use **az deployment group create**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. Asegúrese de que el comando se completa y ProvisioningState es **Correcto**.

1. Confirme que se creó el disco.

     ```sh
     az disk list --output table
     ```
   
## Tarea 5: Implementación de un recurso mediante Azure Bicep

En esta tarea, usará un archivo de Bicep para implementar un disco administrado. Bicep es una herramienta de automatización declarativa que se basa en plantillas de ARM.

1. Continúe trabajando en **Cloud Shell** en una sesión **Bash**.

1. Busque y descargue el archivo **\Allfiles\Lab03\azuredeploydisk.bicep**.

1. **Cargue** el archivo bicep en Cloud Shell. 

1. Seleccione el icono **Editor** (corchetes) y vaya al archivo.

1. Dedique un minuto a leer el archivo de plantilla de bicep. Observe cómo se define el recurso de disco. 
   
1. Haga los siguientes cambios:

    + Cambie el valor **managedDiskName** a `Disk4`.
    + Cambie el valor **nombre de SKU** a `StandardSSD_LRS`.
    + Cambie el valor de **diskSizeinGiB** a `32`.

1. Presione **Ctrl+S** para guardar los cambios.

1. Ahora, implemente la plantilla.

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. Confirme que se creó el disco.

    ```sh
    az disk list --output table
    ```

    >**Nota:** Ha implementado correctamente cinco discos administrados, cada uno de ellos de forma diferente. ¡Buen trabajo!

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot

Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ ¿Cuál es el formato del archivo de plantilla de Azure Resource Manager? Explicar cada componente con ejemplos. 
+ ¿Cómo se usa una plantilla de Azure Resource Manager existente?
+ Compare y contraste las plantillas de Azure Resource Manager y las plantillas de Azure Bicep. 


## Más información con el aprendizaje autodirigido

+ [Implementación de la infraestructura de Azure mediante plantillas de ARM de JSON](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/). Escriba plantillas de Azure Resource Manager de JSON con Visual Studio Code (plantillas de ARM) para implementar la infraestructura en Azure de forma coherente y confiable.
+ [Revisión de las características y herramientas de Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/). Características y herramientas de Cloud Shell. 
+ [Administración de recursos de Azure con Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/). En este módulo se explica cómo instalar los módulos necesarios para la administración de servicios en la nube y usar comandos de PowerShell para realizar tareas administrativas sencillas en recursos en la nube, como máquinas virtuales de Azure, suscripciones de Azure y cuentas de almacenamiento de Azure.
+ [Introducción a Bash](https://learn.microsoft.com/training/modules/bash-introduction/). Use Bash para administrar la infraestructura de TI.
+ [Compilación de la primera plantilla de Bicep](https://learn.microsoft.com/training/modules/build-first-bicep-template/). Defina los recursos de Azure dentro de una plantilla de Bicep. Mejore la coherencia y confiabilidad de las implementaciones, reduzca el esfuerzo manual necesario y escale las implementaciones entre entornos. La plantilla será flexible y reutilizable gracias al uso de parámetros, variables, expresiones y módulos.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Las plantillas de Azure Resource Manager le permiten implementar, administrar y supervisar todos los recursos de la solución como grupo, en lugar de controlar estos recursos individualmente.
+ Una plantilla de Azure Resource Manager es un archivo de notación de objetos JavaScript (JSON) que le permite administrar la infraestructura mediante declaración en lugar de con scripts.
+ En lugar de pasar parámetros como valores insertados en la plantilla, puede usar un archivo JSON independiente que contenga los valores de parámetro.
+ Las plantillas de Azure Resource Manager se pueden implementar de varias maneras, como Azure Portal, Azure PowerShell y la CLI.
+ Bicep es una alternativa a las plantillas de Azure Resource Manager. Bicep usa una sintaxis declarativa para implementar recursos de Azure.
+ Bicep brinda sintaxis concisa, seguridad de tipos confiable y compatibilidad con la reutilización de código. Bicep ofrece la mejor experiencia de creación para sus soluciones de infraestructura como código en Azure.


