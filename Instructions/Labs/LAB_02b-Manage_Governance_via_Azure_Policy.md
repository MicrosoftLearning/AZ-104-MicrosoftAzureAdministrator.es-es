---
lab:
  title: '02b: Administración de la gobernanza mediante Azure Policy'
  module: Administer Governance and Compliance
---

# <a name="lab-02b---manage-governance-via-azure-policy"></a>Laboratorio 02b: Administración de la gobernanza mediante Azure Policy
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

Para mejorar la administración de recursos de Azure en Contoso, se le ha encargado implementar la funcionalidad siguiente:

- Etiquetar grupos de recursos que solo incluyen recursos de infraestructura (por ejemplo, cuentas de almacenamiento de Cloud Shell).

- Asegurarse de que solo se puedan agregar recursos de infraestructura correctamente etiquetados a los grupos de recursos de infraestructura.

- Corregir los recursos no compatibles.

Para obtener una vista previa de este laboratorio en formato de guía interactiva, **[haga clic aquí](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)** .

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderemos a:

+ Tarea 1: Crear y asignar etiquetas a través de Azure Portal
+ Tarea 2: Exigir el etiquetado a través de una instancia de Azure Policy
+ Tarea 3: Aplicar el etiquetado a través de una instancia de Azure Policy

## <a name="estimated-timing-30-minutes"></a>Tiempo estimado: 30 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab02b.png)

## <a name="instructions"></a>Instrucciones

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-assign-tags-via-the-azure-portal"></a>Tarea 1: Asignar etiquetas a través de Azure Portal

En esta tarea, creará y asignará una etiqueta a un grupo de recursos de Azure a través de Azure Portal.

1. En Azure Portal, inicie una sesión de **PowerShell** en **Cloud Shell**.

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**. 

1. En el panel de Cloud Shell, ejecute lo siguiente para identificar el nombre de la cuenta de almacenamiento que usa Cloud Shell:

   ```powershell
   df
   ```

1. En la salida del comando, observe la primera parte de la ruta de acceso completa que designa el montaje de la unidad principal de Cloud Shell (marcado aquí como `xxxxxxxxxxxxxx`:

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. En Azure Portal, busque y seleccione **Cuentas de almacenamiento** y, en la lista de cuentas de almacenamiento, haga clic en la entrada que representa la cuenta de almacenamiento que identificó en el paso anterior.

1. En la hoja de la cuenta de almacenamiento, haga clic en el vínculo que representa el nombre del grupo de recursos que contiene la cuenta de almacenamiento.

    **Nota**: Anote en qué grupo de recursos se encuentra la cuenta de almacenamiento, lo necesitará más adelante en el laboratorio.

1. En la hoja del grupo de recursos, haga clic en **Editar** junto a **Etiquetas** para crear nuevas etiquetas.

1. Cree una etiqueta con la siguiente configuración y aplique el cambio:

    | Configuración | Value |
    | --- | --- |
    | Nombre | **Rol** |
    | Value | **Infraestructura** |

1. Navigate back to the storage account blade. Review the <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> information and note that the new tag was not automatically assigned to the storage account. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Tarea 2: Exigir el etiquetado a través de una instancia de Azure Policy

En esta tarea, asignará la directiva integrada *Requerir una etiqueta y su valor en los recursos* al grupo de recursos y evaluará el resultado. 

1. En Azure Portal, busque y seleccione **Directiva**. 

1. In the <bpt id="p1">**</bpt>Authoring<ept id="p1">**</ept> section, click <bpt id="p2">**</bpt>Definitions<ept id="p2">**</ept>. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> entry (and de-selecting all other entries) in the <bpt id="p2">**</bpt>Category<ept id="p2">**</ept> drop-down list. 

1. Haga clic en la entrada que representa la directiva integrada **Requerir una etiqueta y su valor en los recursos** y revise la definición.

1. En la hoja de la definición de la directiva integrada **Requerir una etiqueta y su valor en los recursos**, haga clic en **Asignar**.

1. Para especificar el **Ámbito**, haga clic en el botón de puntos suspensivos y seleccione los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | Nombre del grupo de recursos que contiene la cuenta de Cloud Shell que identificó en la tarea anterior |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A scope determines the resources or resource groups where the policy assignment takes effect. You could assign policies on the management group, subscription, or resource group level. You also have the option of specifying exclusions, such as individual subscriptions, resource groups, or resources (depending on the assignment scope). 

1. Para configurar las propiedades de nivel **Básico** de la asignación, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre de asignación | **Require Role tag with Infra value** (Requerir etiqueta Rol con valor Infra)|
    | Descripción | **Require Role tag with Infra value for all resources in the Cloud Shell resource group** (Requerir etiqueta Rol con el valor Infra para todos los recursos del grupo de recursos de Cloud Shell)|
    | Aplicación de directivas | habilitado |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Assignment name<ept id="p2">**</ept> is automatically populated with the policy name you selected, but you can change it. You can also add an optional <bpt id="p1">**</bpt>Description<ept id="p1">**</ept>. <bpt id="p1">**</bpt>Assigned by<ept id="p1">**</ept> is automatically populated based on the user name creating the assignment. 

1. Haga clic en **Siguiente** y establezca **Parámetros** en los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Nombre de etiqueta | **Rol** |
    | Valor de etiqueta | **Infraestructura** |

1. Haga clic en **Siguiente** y revise la pestaña **Corrección**. Deje desactivada la casilla **Crear una identidad administrada**. 

    >**Nota:** Esta configuración se puede usar cuando la directiva o la iniciativa incluyen el efecto **deployIfNotExists** o **Modify**.

1. Haga clic en **Revisar + crear** y, después, en **Crear**.

    >**Nota**: Ahora, para comprobar que la nueva asignación de directivas está en vigor, intentará crear otra cuenta de Azure Storage en el grupo de recursos sin agregar explícitamente la etiqueta necesaria. 
    
    >**Nota**: La directiva puede tardar entre 5 y 15 minutos en surtir efecto.

1. Vuelva a la hoja del grupo de recursos que hospeda la cuenta de almacenamiento usada para la unidad principal de Cloud Shell, que identificó en la tarea anterior.

1. En la hoja del grupo de recursos, haga clic en **+ Crear**, busque **Cuenta de almacenamiento** y haga clic en **+ Crear**. 

1. En la pestaña **Aspectos básicos** de la hoja **Crear cuenta de almacenamiento**, compruebe que está usando el grupo de recursos al que se aplicó la directiva y configure las opciones siguientes (deje las demás con sus valores predeterminados), haga clic en **Revisar y crear** y, a continuación, haga clic en **Crear**:

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la cuenta de almacenamiento | Cualquier combinación globalmente única de entre 3 y 24 letras minúsculas y dígitos, empezando por una letra |

1. Once you create the deployment, you should see the <bpt id="p1">**</bpt>Deployment failed<ept id="p1">**</ept> message in the <bpt id="p2">**</bpt>Notifications<ept id="p2">**</ept> list of the portal. From the <bpt id="p1">**</bpt>Notifications<ept id="p1">**</ept> list, navigate to the deployment overview and click the <bpt id="p2">**</bpt>Deployment failed. Click here for details<ept id="p2">**</ept> message to identify the reason for the failure. 

    >**Nota**: Compruebe si el mensaje de error indica que la directiva no ha permitido la implementación de recursos. 

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By clicking the <bpt id="p2">**</bpt>Raw Error<ept id="p2">**</ept> tab, you can find more details about the error, including the name of the role definition <bpt id="p3">**</bpt>Require Role tag with Infra value<ept id="p3">**</ept>. The deployment failed because the storage account you attempted to create did not have a tag named <bpt id="p1">**</bpt>Role<ept id="p1">**</ept> with its value set to <bpt id="p2">**</bpt>Infra<ept id="p2">**</ept>.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Tarea 3: Aplicar el etiquetado a través de una instancia de Azure Policy

En esta tarea, usaremos una definición de directiva diferente para corregir los recursos no compatibles. 

1. En Azure Portal, busque y seleccione **Directiva**. 

1. En la sección **Creación**, haga clic en **Asignaciones**. 

1. En la lista de asignaciones, haga clic en el icono de puntos suspensivos de la fila que representa la asignación de directiva etiqueta **Require Role tag with Infra value** (Requerir etiqueta Rol con valor Infra) y use el elemento de menú **Eliminar asignación** para eliminar la asignación.

1. Haga clic en **Asignar directiva** y, para especificar el **Ámbito**, haga clic en el botón de puntos suspensivos y seleccione los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | Nombre del grupo de recursos que contiene la cuenta de Cloud Shell que identificó en la primera tarea |

1. Para especificar la **Definición de directiva**, haga clic en el botón de puntos suspensivos y busque y seleccione **Heredar una etiqueta del grupo de recursos si falta**.

1. Para configurar las propiedades restantes de nivel **Básico** de la asignación, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre de asignación | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing** (Heredar la etiqueta Rol y su valor Infra del grupo de recursos de Cloud Shell si falta)|
    | Descripción | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing** (Heredar la etiqueta Rol y su valor Infra del grupo de recursos de Cloud Shell si falta)|
    | Aplicación de directivas | habilitado |

1. Haga clic en **Siguiente** y establezca **Parámetros** en los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Nombre de etiqueta | **Rol** |

1. Haga clic en **Siguiente** y, en la pestaña **Corrección**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Crear una tarea de corrección | enabled |
    | Directiva que se debe corregir | **Heredar una etiqueta de la suscripción si falta** |

    >**Nota**: La definición de esta directiva incluye el efecto **Modify**.

1. Haga clic en **Revisar + crear** y, después, en **Crear**.

    >**Nota**: Para comprobar que la nueva asignación de directivas está en vigor, creará otra cuenta de Azure Storage en el mismo grupo de recursos sin agregar explícitamente la etiqueta necesaria. 
    
    >**Nota**: La directiva puede tardar entre 5 y 15 minutos en surtir efecto.

1. Vuelva a la hoja del grupo de recursos que hospeda la cuenta de almacenamiento usada para la unidad principal de Cloud Shell, que identificó en la primera tarea.

1. En la hoja del grupo de recursos, haga clic en **+ Crear**, busque **Cuenta de almacenamiento** y haga clic en **+ Crear**. 

1. En la pestaña **Aspectos básicos** de la hoja **Crear cuenta de almacenamiento**, compruebe que está usando el grupo de recursos al que se aplicó la directiva y configure las opciones siguientes (deje las demás con sus valores predeterminados) y haga clic en **Revisar y crear**:

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la cuenta de almacenamiento | Cualquier combinación globalmente única de entre 3 y 24 letras minúsculas y dígitos, empezando por una letra |

1. Compruebe que esta vez se haya superado la validación y haga clic en **Crear**.

1. Una vez que se aprovisione la nueva cuenta de almacenamiento, haga clic en el botón **Ir al recurso** y, en la hoja **Información general** de la cuenta de almacenamiento recién creada, observe que la etiqueta **Rol** con el valor **Infra** se ha asignado automáticamente al recurso.

#### <a name="task-4-clean-up-resources"></a>Tarea 4: Limpieza de recursos

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges, although keep in mind that Azure policies do not incur extra cost.
   
   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. En el portal, busque y seleccione **Directiva**.

1. En la sección **Creación**, haga clic en **Asignaciones**, haga clic en el icono de puntos suspensivos situado a la derecha de la asignación que creó en la tarea anterior y haga clic en **Eliminar asignación**. 

1. En el portal, busque y seleccione **Cuentas de almacenamiento**.

1. In the list of storage accounts, select the resource group corresponding to the storage account you created in the last task of this lab. Select <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> (Trash can to the right) to the <bpt id="p3">**</bpt>Role:Infra<ept id="p3">**</ept> tag and press <bpt id="p4">**</bpt>Apply<ept id="p4">**</ept>. 

1. Click <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> on the top of the storage account blade. When prompted for the confirmation, in the <bpt id="p1">**</bpt>Delete storage account<ept id="p1">**</ept> blade, type the name of the storage account to confirm and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept>. 

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Creado y asignado etiquetas a través de Azure Portal
- Exigido el etiquetado a través de una instancia de Azure Policy
- Aplicado el etiquetado a través de una instancia de Azure Policy
