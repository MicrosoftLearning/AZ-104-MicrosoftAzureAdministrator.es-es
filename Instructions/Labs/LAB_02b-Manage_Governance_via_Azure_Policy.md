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

                **Nota:** Hay disponible una **[simulación de laboratorio interactiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)** que le permite realizar sus propias selecciones a su entera discreción. Es posible que encuentre pequeñas diferencias entre la simulación interactiva y el laboratorio hospedado, pero las ideas y los conceptos básicos que se muestran son los mismos. 

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

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **Rol** |
    | Value | **Infraestructura** |

1. Vuelva a la hoja de la cuenta de almacenamiento. Revise la **Información general** y observe que la nueva etiqueta no se asignó automáticamente a la cuenta de almacenamiento. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Tarea 2: Exigir el etiquetado a través de una instancia de Azure Policy

En esta tarea, asignará la directiva integrada *Requerir una etiqueta y su valor en los recursos* al grupo de recursos y evaluará el resultado. 

1. En Azure Portal, busque y seleccione **Directiva**. 

1. En la sección **Creación**, haga clic en **Definiciones**. Tómese un momento para examinar la lista de definiciones de directivas integradas que están disponibles para usar. Para enumerar todas las directivas integradas que implican el uso de etiquetas, seleccione la entrada **Etiquetas** (y anule la selección de todas las demás entradas) en la lista desplegable **Categoría**. 

1. Haga clic en la entrada que representa la directiva integrada **Requerir una etiqueta y su valor en los recursos** y revise la definición.

1. En la hoja de la definición de la directiva integrada **Requerir una etiqueta y su valor en los recursos**, haga clic en **Asignar**.

1. Para especificar el **Ámbito**, haga clic en el botón de puntos suspensivos y seleccione los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | Nombre del grupo de recursos que contiene la cuenta de Cloud Shell que identificó en la tarea anterior |

    >**Nota**: Un ámbito determina los recursos o grupos de recursos en los que la asignación de directivas tiene efecto. Puede asignar directivas a nivel de grupo de administración, de suscripción o de grupo de recursos. También tiene la opción de especificar exclusiones, como suscripciones, grupos de recursos o recursos individuales (según el ámbito de asignación). 

1. Para configurar las propiedades de nivel **Básico** de la asignación, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre de asignación | **Require Role tag with Infra value** (Requerir etiqueta Rol con valor Infra)|
    | Descripción | **Require Role tag with Infra value for all resources in the Cloud Shell resource group** (Requerir etiqueta Rol con el valor Infra para todos los recursos del grupo de recursos de Cloud Shell)|
    | Aplicación de directivas | habilitado |

    >**Nota**: El **Nombre de asignación** se rellena automáticamente con el nombre de directiva seleccionado, pero puede cambiarlo. También puede agregar una **Descripción** opcional. **Asignado por** se rellena automáticamente en función del nombre de usuario que crea la asignación. 

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

1. Una vez que cree la implementación, verá el mensaje **Error de implementación** en la lista **Notificaciones** del portal. En la lista **Notificaciones**, vaya a la información general de la implementación y haga clic en el mensaje **Error en la implementación. Haga clic aquí para ver los detalles** para conocer el motivo del error. 

    >**Nota**: Compruebe si el mensaje de error indica que la directiva no ha permitido la implementación de recursos. 

    >**Nota**: Al hacer clic en la pestaña **Error sin procesar**, puede encontrar más detalles sobre el error, incluido el nombre de la definición de rol **Require Role tag with Infra value** (Requerir etiqueta Rol con el valor Infra). El error en la implementación se debe a que la cuenta de almacenamiento que intentó crear no tenía una etiqueta denominada **Rol** con su valor establecido en **Infra**.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Tarea 3: Aplicar el etiquetado a través de una instancia de Azure Policy

En esta tarea, usaremos una definición de directiva diferente para corregir los recursos no compatibles. 

1. En Azure Portal, busque y seleccione **Directiva**. 

1. En la sección **Creación**, haga clic en **Asignaciones**. 

1. En la lista de asignaciones, haga clic en el icono de puntos suspensivos de la fila que representa la asignación de directiva etiqueta **Require Role tag with Infra value** (Requerir etiqueta Rol con valor Infra) y use el elemento de menú **Eliminar asignación** para eliminar la asignación.

1. Haga clic en **Asignar directiva** y, para especificar el **Ámbito**, haga clic en el botón de puntos suspensivos y seleccione los valores siguientes:

    | Configuración | Valor |
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
    | Directiva que se debe corregir | **Heredar una etiqueta del grupo de recursos si falta** |

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

   >**Nota**: No olvide quitar los recursos de Azure recién creados que ya no use. La eliminación de recursos no utilizados garantiza que no aparezcan cargos inesperados, aunque recuerde que las directivas de Azure no incurren en costos adicionales.
   
   >**Nota:** No se preocupe si los recursos del laboratorio no se pueden quitar inmediatamente. A veces, los recursos tienen dependencias y se tarda más tiempo en eliminarlos. Supervisar el uso de los recursos es una tarea habitual del administrador, así que solo tiene que revisar periódicamente los recursos en el portal para ver cómo va la limpieza. 

1. En el portal, busque y seleccione **Directiva**.

1. En la sección **Creación**, haga clic en **Asignaciones**, haga clic en el icono de puntos suspensivos situado a la derecha de la asignación que creó en la tarea anterior y haga clic en **Eliminar asignación**. 

1. En el portal, busque y seleccione **Cuentas de almacenamiento**.

1. En la lista de cuentas de almacenamiento, seleccione el grupo de recursos correspondiente a la cuenta de almacenamiento que creó en la última tarea de este laboratorio. Seleccione **Etiquetas** y haga clic en **Eliminar** (papelera a la derecha) en la etiqueta **Rol:Infra** y presione **Aplicar**. 

1. Haga clic en **Información general** y elija **Eliminar** en la parte superior de la hoja de la cuenta de almacenamiento. Cuando se le pida confirmación, en la hoja **Eliminar cuenta de almacenamiento**, escriba el nombre de la cuenta de almacenamiento para confirmarla y haga clic en **Eliminar**. 

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Creado y asignado etiquetas a través de Azure Portal
- Exigido el etiquetado a través de una instancia de Azure Policy
- Aplicado el etiquetado a través de una instancia de Azure Policy
