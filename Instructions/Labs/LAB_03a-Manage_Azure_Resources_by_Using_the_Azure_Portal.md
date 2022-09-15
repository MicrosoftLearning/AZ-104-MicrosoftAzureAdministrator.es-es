---
lab:
  title: '03a: Administración de recursos de Azure con Azure Portal'
  module: Administer Azure Resources
---

# <a name="lab-03a---manage-azure-resources-by-using-the-azure-portal"></a>Laboratorio 03a: Administración de recursos de Azure con Azure Portal
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

You need to explore the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups, including moving resources between resource groups. You also want to explore options for protecting disk resources from being accidentally deleted, while still allowing for modifying their performance characteristics and size.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderemos a:

+ Tarea 1: Crear grupos de recursos e implementar recursos en ellos
+ Tarea 2: Mover recursos entre grupos de recursos
+ Tarea 3: Implementar y probar los bloqueos de recursos

## <a name="estimated-timing-20-minutes"></a>Tiempo estimado: 20 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab03a.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-create-resource-groups-and-deploy-resources-to-resource-groups"></a>Tarea 1: Crear grupos de recursos e implementar recursos en ellos

En esta tarea, usará Azure Portal para crear grupos de recursos y crear un disco en el grupo de recursos.

1. Inicie sesión en [**Azure Portal**](http://portal.azure.com).

1. En Azure Portal, busque y seleccione **Discos**, haga clic en **+ Crear** y especifique los siguientes valores:

    |Configuración|Valor|
    |---|---|
    |Suscripción| Nombre de la suscripción de Azure donde creó el grupo de recursos |
    |Grupo de recursos| Nombre de un nuevo grupo de recursos **az104-03a-rg1** |
    |Nombre del disco| **az104-03a-disk1** |
    |Region| **(EE. UU.) Este de EE. UU.** |
    |Zona de disponibilidad| **None** |
    |Tipo de origen| **None** |

    >**Nota**: Al crear un recurso, tiene la opción de crear un grupo de recursos o usar uno existente.

1. Cambie el tipo y tamaño de disco a **HDD estándar** y **32 GiB**, respectivamente.

1. Haga clic en **Revisar + crear** y, después, en **Crear**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait until the disk is created. This should take less than a minute.

#### <a name="task-2-move-resources-between-resource-groups"></a>Tarea 2: Mover recursos entre grupos de recursos 

En esta tarea, moveremos el recurso de disco que creó en la tarea anterior a un nuevo grupo de recursos. 

1. Busque y seleccione **Grupos de recursos**. 

1. En la hoja **Grupos de  recursos**, haga clic en la entrada que representa el grupo de recursos **az104-03a-rg1** que creó en la tarea anterior.

1. En la hoja **Información general** del grupo de recursos, en la lista de recursos del grupo de recursos, seleccione la entrada que representa el disco recién creado, haga clic en **Mover** en la barra de herramientas y, en la lista desplegable, seleccione **Mover a otro grupo de recursos**.

    >**Nota**: Este método permite mover varios recursos al mismo tiempo. 

1. Below the <bpt id="p1">**</bpt>Resource group<ept id="p1">**</ept> text box, click <bpt id="p2">**</bpt>Create new<ept id="p2">**</ept> then type <bpt id="p3">**</bpt>az104-03a-rg2<ept id="p3">**</ept> in the text box. On the Review tab, select the checkbox <bpt id="p1">**</bpt>I understand that tools and scripts associated with moved resources will not work until I update them to use new resource IDs<ept id="p1">**</ept>, and click <bpt id="p2">**</bpt>Move<ept id="p2">**</ept>.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the move to complete but instead proceed to the next task. The move might take about 10 minutes. You can determine that the operation was completed by monitoring activity log entries of the source or target resource group. Revisit this step once you complete the next task.

#### <a name="task-3-implement-resource-locks"></a>Tarea 3: Implementar bloqueos de recursos

En esta tarea, aplicará un bloqueo de recursos a un grupo de recursos de Azure que contenga un recurso de disco.

1. En Azure Portal, busque y seleccione **Discos**, haga clic en **+ Crear** y especifique los siguientes valores:

    |Configuración|Valor|
    |---|---|
    |Suscripción| Nombre de la suscripción que está usando en este laboratorio |
    |Grupo de recursos| Haga clic en **Crear nuevo** grupo de recursos y asígnele el nombre **az104-03a-rg3** |
    |Nombre del disco| **az104-03a-disk2** |
    |Region| Nombre de la región de Azure donde creó los otros grupos de recursos en este laboratorio |
    |Zona de disponibilidad| **None** |
    |Tipo de origen| **None** |

1. Establezca el tipo y tamaño de disco a **HDD estándar** y **32 GiB**, respectivamente.

1. Haga clic en **Revisar + crear** y, después, en **Crear**.

1. Haga clic en **Ir al recurso**.

1. En la página Información general del disco, haga clic en el nombre del grupo de recursos, **az104-03a-rg3**.

1. En la hoja del grupo de recursos **az104-03a-rg3**, haga clic en **Bloqueos** y luego en **+ Agregar** y configure las opciones siguientes:

    |Configuración|Valor|
    |---|---|
    |Nombre del bloqueo| **az104-03a-delete-lock** |
    |Tipo de bloqueo| **Eliminar** |
    
1. Haga clic en **Aceptar**    

1. En la hoja del grupo de recursos **az104-03a-rg3**, haga clic en **Información general**, en la lista de recursos del grupo de recursos, seleccione la entrada que representa el disco que creó anteriormente en esta tarea y haga clic en **Eliminar** en la barra de herramientas. 

1. Cuando se le pregunte **¿Quiere eliminar todos los recursos seleccionados?** , en el cuadro de texto **Confirmar eliminación**, escriba **sí** y haga clic en **Eliminar**.

1. Debería ver un mensaje de error que notifica el error de la operación de eliminación. 

    >**Nota**: Como indica el mensaje de error, esto es esperable debido al bloqueo de eliminación aplicado a nivel del grupo de recursos.

1. Vuelva a la lista de recursos del grupo de recursos **az104-03a-rg3** y haga clic en la entrada que representa el recurso **az104-03a-disk2**. 

1. Tiene que explorar las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos, incluido el movimiento de recursos entre grupos de recursos.

    >**Nota**: Esto es lo esperado, ya que el bloqueo a nivel de grupo de recursos solo se aplica a las operaciones de eliminación. 

#### <a name="clean-up-resources"></a>Limpieza de recursos

   >También quiere explorar las opciones para proteger los recursos de disco frente a la eliminación accidental, a la vez que se permita modificar las características de rendimiento y tamaño.

1. Vaya a la hoja del grupo de recursos **az104-03a-rg3**, muestre su hoja **Bloqueos** y quite el bloqueo **az104-03a-delete-lock** haciendo clic en el vínculo **Eliminar** en el lado derecho de la entrada de bloqueo **Eliminar**.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Creado grupos de recursos e implementado recursos en ellos
- Movido recursos entre grupos de recursos
- Implementado y probado bloqueos de recursos
