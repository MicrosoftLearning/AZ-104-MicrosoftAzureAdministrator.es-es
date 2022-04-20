---
lab:
  title: '03b: Administración de recursos de Azure mediante plantillas de ARM'
  module: Module 03 - Azure Administration
ms.openlocfilehash: 602da542fdf20f6b1be637e792ec47daaa0de04b
ms.sourcegitcommit: 8282cbcee5f7cd46bdc73d781c460d6a078049bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2022
ms.locfileid: "143611555"
---
# <a name="lab-03b---manage-azure-resources-by-using-arm-templates"></a>Laboratorio 03b: Administración de recursos de Azure mediante plantillas de ARM
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio
Ahora que ha explorado las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos mediante Azure Portal, debe llevar a cabo la tarea equivalente mediante plantillas de Azure Resource Manager.

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Revisar una plantilla de ARM para la implementación de un disco administrado de Azure
+ Tarea 2: Crear un disco administrado de Azure mediante una plantilla de ARM
+ Tarea 3: Revisar la implementación del disco administrado basada en la plantilla de ARM

## <a name="estimated-timing-20-minutes"></a>Tiempo estimado: 20 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab03b.png)

## <a name="instructions"></a>Instrucciones

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-review-an-arm-template-for-deployment-of-an-azure-managed-disk"></a>Tarea 1: Revisar una plantilla de ARM para la implementación de un disco administrado de Azure

En esta tarea, creará un recurso de disco de Azure mediante una plantilla de Azure Resource Manager.

1. Inicie sesión en [**Azure Portal**](http://portal.azure.com).

1. En Azure Portal, busque y seleccione **Grupos de recursos**. 

1. En la lista de grupos de recursos, haga clic en **az104-03a-rg1**.

1. En la hoja del grupo de recursos **az104-03a-rg1**, en la sección **Configuración**, haga clic en **Implementaciones**.

1. En la hoja **az104-03a-rg1 - Implementaciones**, haga clic en la primera entrada de la lista de implementaciones.

1. En la hoja **Microsoft.ManagedDisk-* XXXXXXXXX* \| Información general**, haga clic en **Plantilla**.

    >**Nota:** Revise el contenido de la plantilla y tenga en cuenta que tiene la opción **descargarla** en el equipo local, **agregarla a la biblioteca** o **implementarla** de nuevo.

1. Haga clic en **Descargar** y guarde el archivo comprimido que contiene los archivos de parámetros y plantilla en la carpeta **Descargas** del equipo de laboratorio.

1. En la hoja **Microsoft.ManagedDisk-* XXXXXXXXX* \| Plantilla**, haga clic en **Entradas**.

1. Anote el valor del parámetro **ubicación**. Lo necesitará en la próxima tarea.

1. Extraiga el contenido del archivo descargado en la carpeta **Descargas** del equipo de laboratorio.

    >**Nota:** Estos archivos también están disponibles como **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** y **\\Allfiles\\Labs\\03\\az104-03b-md-parameters.json**.
    
1. Cierre todas las ventanas del **Explorador de archivos**.

#### <a name="task-2-create-an-azure-managed-disk-by-using-an-arm-template"></a>Tarea 2: Crear un disco administrado de Azure mediante una plantilla de ARM

1. En Azure Portal, busque y seleccione **Implementar una plantilla personalizada**.

1. En la hoja **Implementación personalizada**, haga clic en **Cree su propia plantilla en el editor**.

1. En la hoja **Editar plantilla**, haga clic en **Cargar archivo** y cargue el archivo **template.json** que descargó en la tarea anterior.

1. En el panel del editor, quite las líneas siguientes:

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   "sourceUri": {
       "type": "String"
   },
   "osType": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

   ```json
   "osType": "[parameters('osType')]",
   ```

    >**Nota**: Estos parámetros se quitan porque no son aplicables a la implementación actual. En concreto, los parámetros sourceResourceId, sourceUri, osType y hyperVGeneration son aplicables a la creación de un disco de Azure a partir de un archivo VHD existente.

1. **Guarde** los cambios.

1. De nuevo en la hoja **Implementación personalizada**, haga clic en **Editar parámetros**. 

1. En la hoja **Editar parámetros**, haga clic en **Cargar archivo** y cargue el archivo **parameters.json** que descargó en la tarea anterior, y haga clic en **Guardar** los cambios.

1. De nuevo en la hoja **Implementación personalizada**, configure las opciones siguientes:

    | Configuración | Valor |
    | --- |--- |
    | Subscription | *Nombre de la suscripción de Azure que está usando en este laboratorio* |
    | Grupo de recursos | Nombre de un **nuevo** grupo de recursos **az104-03b-rg1** |
    | Region | Nombre de cualquier región de Azure disponible en la suscripción que está usando en este laboratorio |
    | Nombre del disco | **az104-03b-disk1** |
    | Location | Valor del parámetro de ubicación que anotó en la tarea anterior |
    | SKU | **Standard_LRS** |
    | Tamaño del disco en GB | **32** |
    | Create Option (Opción de creación) | **empty** |
    | Disk Encryption Set Type (Tipo de conjunto de cifrado de disco) | **EncryptionAtRestWithPlatformKey** |
    | Directiva de acceso a la red | **AllowAll** |

1. Seleccione **Revisar y crear** y, luego, **Crear**.

1. Compruebe que la implementación se haya completado correctamente.

#### <a name="task-3-review-the-arm-template-based-deployment-of-the-managed-disk"></a>Tarea 3: Revisar la implementación del disco administrado basada en la plantilla de ARM

1. En Azure Portal, busque y seleccione **Grupos de recursos**. 

1. En la lista de grupos de recursos, haga clic en **az104-03b-rg1**.

1. En la hoja del grupo de recursos **az104-03b-rg1**, en la sección **Configuración**, haga clic en **Implementaciones**.

1. En la hoja **az104-03b-rg1 - Implementaciones**, haga clic en la primera entrada de la lista de implementaciones y revise el contenido de las hojas **Entrada** y **Plantilla**.

#### <a name="clean-up-resources"></a>Limpieza de recursos

   >**Nota**: No elimine los recursos que implementó en este laboratorio. Hará referencia a ellos en el siguiente laboratorio de este módulo.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Revisado una plantilla de ARM para la implementación de un disco administrado de Azure
- Creado un disco administrado de Azure mediante una plantilla de ARM
- Revisado la implementación del disco administrado basada en la plantilla de ARM
