---
lab:
  title: '03b: Administración de recursos de Azure mediante plantillas de ARM'
  module: Administer Azure Resources
---

# <a name="lab-03b---manage-azure-resources-by-using-arm-templates"></a>Laboratorio 03b: Administración de recursos de Azure mediante plantillas de ARM
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio
Ahora que ha explorado las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos mediante Azure Portal, debe llevar a cabo la tarea equivalente mediante plantillas de Azure Resource Manager.

Para obtener una vista previa de este laboratorio en formato de guía interactiva, **[haga clic aquí](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)** .

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

1. Note the value of the <bpt id="p1">**</bpt>location<ept id="p1">**</ept> parameter. You will need it in the next task.

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
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: These parameters are removed since they are not applicable to the current deployment. In particular, sourceResourceId, sourceUri, osType, and hyperVGeneration parameters are applicable to creating an Azure disk from an existing VHD file.

1. **Guarde** los cambios.

1. De nuevo en la hoja **Implementación personalizada**, haga clic en **Editar parámetros**. 

1. En la hoja **Editar parámetros**, haga clic en **Cargar archivo** y cargue el archivo **parameters.json** que descargó en la tarea anterior, y haga clic en **Guardar** los cambios.

1. De nuevo en la hoja **Implementación personalizada**, configure las opciones siguientes:

    | Configuración | Value |
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

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Revisado una plantilla de ARM para la implementación de un disco administrado de Azure
- Creado un disco administrado de Azure mediante una plantilla de ARM
- Revisado la implementación del disco administrado basada en la plantilla de ARM
