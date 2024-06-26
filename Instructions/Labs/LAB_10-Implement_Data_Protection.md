---
lab:
  title: "Laboratorio\_10: Implementación de la protección de datos"
  module: Administer Data Protection
---

# Laboratorio 10: Implementación de la protección de datos

## Introducción al laboratorio    

En este laboratorio, obtendrá información sobre la copia de seguridad y la recuperación de máquinas virtuales de Azure. Aprenderá a crear un almacén de Recovery Service y una directiva de copia de seguridad para máquinas virtuales de Azure. Obtendrá información sobre la recuperación ante desastres con Azure Site Recovery. 

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción podría afectar a la disponibilidad de las características de este laboratorio. Puede cambiar las regiones, pero los pasos se escriben teniendo en cuenta las regiones **Este de EE. UU.** y **Oeste de EE. UU.**

## Tiempo estimado: 50 minutos

## Escenario del laboratorio

Su organización está evaluando cómo realizar copias de seguridad y restaurar máquinas virtuales de Azure a partir de una pérdida de datos accidental o malintencionada. Además, la organización quiere explorar el uso de Azure Site Recovery para escenarios de recuperación ante desastres. 

## Simulación interactiva de laboratorio

Hay una simulación de laboratorio interactiva que puede resultar útil para este tema. La simulación permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure.

+ **[Copia de seguridad de máquinas virtuales y archivos locales.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)**. Cree un almacén de Recovery Services e implemente una copia de seguridad de máquina virtual de Azure. Implemente la copia de seguridad local de carpetas y archivos mediante el agente de Microsoft Azure Recovery Services. Las copias de seguridad locales están fuera del ámbito de este laboratorio, pero puede resultar útil ver esos pasos. 

## Aptitudes de trabajo

+ Tarea 1: Use una plantilla para aprovisionar una infraestructura.
+ Tarea 2: Creación y configuración de un almacén de Recovery Services.
+ Tarea 3: Configure la copia de seguridad de nivel de máquina virtual de Azure.
+ Tarea 4: Supervise Azure Backup.
+ Tarea 5: Habilite la replicación de máquinas virtuales. 

## Tiempo estimado: 40 minutos

## Diagrama de la arquitectura

![Diagrama de las tareas de arquitectura.](../media/az104-lab10-architecture.png)

## Tarea 1: Uso de una plantilla para aprovisionar una infraestructura

En esta tarea, usará una plantilla para implementar una máquina virtual. La máquina virtual se usará para probar distintos escenarios de copia de seguridad.

1. Descargue los archivos de laboratorio de **\\Allfiles\\Lab10\\**.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `Deploy a custom template`.

1. En la página de implementación personalizada, seleccione **Crear plantilla propia en el editor**.

1. En la página de edición de la plantilla, seleccione **Cargar archivo**.

1. Busque y seleccione el archivo **\\Allfiles\\Lab10\\az104-10-vms-edge-template.json** y seleccione **Abrir**.

   >**Nota:** Dedique un momento a revisar la plantilla. Estamos implementando una red virtual y una máquina virtual para poder mostrar la copia de seguridad y la recuperación. 

1. Guarde los cambios mediante **Guardar**.

1. Seleccione **Editar parámetros** y, a continuación, **Cargar archivo**.

1. Cargue y seleccione el archivo **\\Allfiles\\Lab10\\az104-10-vms-edge-parameters.json**.

1. Guarde los cambios mediante **Guardar**.

1. Use la siguiente información para completar los campos de implementación personalizados y deje todos los demás campos con sus valores predeterminados:

    | Configuración       | Valor         | 
    | ---           | ---           |
    | Subscription  | Su suscripción de Azure |
    | Resource group| `az104-rg-region1` (si fuera necesario, seleccione **Crear nuevo**)
    | Region        | **Este de EE. UU.**   |
    | Nombre de usuario      | **localadmin**   |
    | Contraseña      | Especifique una contraseña compleja |

1. Seleccione **Revisar y crear** y, a continuación, seleccione **Crear**.

    >**Nota:** Espere a que se implemente la plantilla y seleccione **Ir al recurso**. Debe tener una máquina virtual en una red virtual. 

## Tarea 2: Creación y configuración de un almacén de Recovery Services

En esta tarea, creará un almacén de Recovery Services. Un almacén de Recovery Services proporciona almacenamiento para los datos de la máquina virtual. 

1. En Azure Portal, busque y seleccione `Recovery Services vaults` y, en la hoja **Almacenes de Recovery Services**, haga clic en **+ Crear**.

1. En la hoja **Create Recovery Services vault** (Crear almacén de Recovery Services), configure las opciones siguientes:

    | Configuración | Value |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure |
    | Resource group | `az104-rg-region1`  |
    | Nombre del almacén | `az104-rsv-region1` |
    | Region | **Este de EE. UU.** |

    >**Nota**: Asegúrese de especificar la misma región en la que implementó las máquinas virtuales en la tarea anterior.

    ![Captura de pantalla del almacén de Recovery Services.](../media/az104-lab10-create-rsv.png)

1. Haga clic en **Revisar y crear**, asegúrese de que se haya superado la validación y, a continuación, haga clic en **Crear**.

    >**Nota**: Espere a que la implementación se complete. La implementación podría tardar un par de minutos. 

1. Cuando se haya completado la implementación, haga clic en **Ir al recurso**.

1. En la sección **Configuración**, haga clic en **Propiedades**.

1. Seleccione el vínculo **Actualizar** en la etiqueta **Configuración de copia de seguridad**.

1. En la hoja **Configuración de copia de seguridad**, revisa las opciones de **Tipo de replicación de almacenamiento**. Deje establecido el valor predeterminado de **Redundancia geográfica** y cierre la hoja.

    >**Nota**: Esta opción solo se puede configurar si no hay elementos de copia de seguridad existentes.
    
    >**¿Sabía que...?** La opción "Restauración entre regiones (CRR)" permite restaurar datos en una [región de Azure emparejada](https://learn.microsoft.com/azure/backup/backup-create-recovery-services-vault#set-cross-region-restore) secundaria. 

1. Seleccione el vínculo **Actualizar** en la etiqueta **Configuración de seguridad > configuración de seguridad y eliminación temporal**.

1. En la hoja **Configuración de seguridad**, tenga en cuenta que la opción **Eliminación temporal (para cargas de trabajo que se ejecutan en Azure)**  está **habilitada**. Fíjese en que el **período de retención de eliminación temporal** es de **14** días. 

>**¿Sabía que...?** Azure tiene dos tipos de almacenes: Almacenes de Recovery Services y almacenes de Backup La principal diferencia reside en los orígenes de datos de los que se puede realizar una copia de seguridad. Obtenga más información sobre [las diferencias](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault).

## Tarea 3: Configuración de la copia de seguridad de nivel de máquina virtual

En esta tarea, implementará la copia de seguridad a nivel de máquina virtual de Azure. Como parte de una copia de seguridad de máquina virtual, deberá definir la directiva de copia de seguridad y retención que se aplica a la copia de seguridad. Las distintas máquinas virtuales pueden tener diferentes directivas de copia de seguridad y retención asignadas.

   >**Nota**: Antes de comenzar esta tarea, asegúrese de que la implementación que inició en la primera tarea de este laboratorio se haya completado correctamente.

1. En la hoja del almacén de Recovery Services, haga clic en **Información general** y, a continuación, haga clic en **+ Copia de seguridad**.

1. En la hoja **Objetivo de Backup**, configure las opciones siguientes:

    | Configuración | Value |
    | --- | --- |
    | ¿Dónde se ejecuta su carga de trabajo? | **Azure** (vea las otras opciones) |
    | ¿De qué quiere hacer una copia de seguridad? | **Máquina virtual** (vea las otras opciones) |

1. Seleccione **Backup** (Hacer copia de seguridad).

1. Observe que hay dos **subtipos de directiva**: **Mejorada** y **Estándar**. Revise las opciones y seleccione **Estándar**. 

1. En **Directiva de copia de seguridad**, seleccione **Crear una directiva**.

1. Defina una nueva directiva de copia de seguridad con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | ---- | ---- |
    | Nombre de la directiva | `az104-backup` |
    | Frecuencia | **A diario** |
    | Time | **12:00 a. m.** |
    | Zona horaria | Nombre de la zona horaria local |
    | Conservar las instantáneas de recuperación instantánea durante | **2** días |

    ![Captura de pantalla de la página de directiva de copia de seguridad.](../media/az104-lab10-backup-policy.png)

1. Haga clic en **Aceptar** para crear la directiva y luego, en la sección **Máquinas virtuales**, seleccione **Añadir**.

1. En la hoja **Seleccionar máquinas virtuales**, seleccione **az-104-10-vm0**, haga clic en **Aceptar** y vuelva a la hoja **Copia de seguridad**; después, haga clic en **Habilitar copia de seguridad**.

    >**Nota**: Espere a que se habilite la copia de seguridad. Esta operación debe durar unos 2 minutos.

1. En la sección **Elementos protegidos**, haga clic en **Elementos de copia de seguridad** y, a continuación, haga clic en la entrada **máquina virtual** de Azure.

1. Seleccione el vínculo **Ver detalles** para **az104-10-vm0** y revise los valores de las entradas **Comprobación previa de copia de seguridad** y **último estado de copia de seguridad**.

    >**Nota:** Fíjese en que la copia de seguridad está pendiente.
    
1. Seleccione **Copia de seguridad ahora**, acepte el valor predeterminado en la lista desplegable **Conservar copia de seguridad hasta** y haga clic en **Aceptar**.

    >**Nota**: No espere a que se complete la copia de seguridad, sino que avance a la siguiente tarea.

## Tarea 4: Supervisión de Azure Backup

En esta tarea, implementará una cuenta de almacenamiento de Azure. A continuación, configurará el almacén para enviar los registros y las métricas a la cuenta de almacenamiento. A continuación, este repositorio se puede usar con Log Analytics u otras soluciones de supervisión de terceros.

1. En Azure Portal, busque y seleccione `Storage accounts`.

1. En la página Cuentas de almacenamiento, seleccione **Crear**.

1. Use la siguiente información para definir la cuenta de almacenamiento y seleccione **Revisar**.

    | Configuración | Value |
    | --- | --- | 
    | Suscripción          | *Su suscripción*    |
    | Resource group        | **az104-rg-region1**        |
    | Nombre de la cuenta de almacenamiento  | Proporcione un nombre único global.   |
    | Region                | **Este de EE. UU.**   |

1. En la pestaña Revisión, seleccione **Crear**.

    >**Nota**: Espere a que la implementación se complete. Esta operación debería tardar aproximadamente un minuto.

1. Busque y seleccione el almacén de Recovery Services.

1. Seleccione **Configuración de diagnóstico** y, a continuación, seleccione **Agregar configuración de diagnóstico**.

1. Asigne a la configuración el nombre `Logs and Metrics to storage`.

1. Coloque una marca de verificación junto a las siguientes categorías de registro y métricas:

    - **Datos de informes de Azure Backup**
    - **Datos de trabajo de Azure Backup del complemento**
    - **Datos de alerta de Azure Backup del complemento**
    - **Trabajos de Azure Site Recovery**
    - **Eventos de Azure Site Recovery**
    - **Salud**

1. En Detalles de destino, coloque una marca de verificación junto a **Archivar en una cuenta de almacenamiento**.

1. En el campo desplegable Cuenta de almacenamiento, seleccione la cuenta de almacenamiento que implementó anteriormente en esta tarea.

1. Seleccione **Guardar**.

1. Vuelva al almacén de Recovery Services y, en la hoja **Supervisión**, seleccione **Trabajos de copia de seguridad**.

1. Busque la operación de copia de seguridad de la máquina virtual **az104-10-vm0**. 

1. Revise los detalles del trabajo de copia de seguridad.

## Tarea 5: Habilitar la replicación de máquinas virtuales

1. En Azure Portal, busque y seleccione `Recovery Services vaults` y, en la hoja **Almacenes de Recovery Services**, haga clic en **+ Crear**.

1. En la hoja **Create Recovery Services vault** (Crear almacén de Recovery Services), configure las opciones siguientes:

    | Configuración | Value |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure |
    | Resource group | `az104-rg-region2` (Si es necesario, seleccione **Crear nuevo**) |
    | Nombre del almacén | `az104-rsv-region2` |
    | Region | **Oeste de EE. UU.** |

    >**Nota**: Asegúrese de especificar una **región diferente** a la máquina virtual.

1. Haga clic en **Revisar y crear**, asegúrese de que se haya superado la validación y, a continuación, haga clic en **Crear**.

    >**Nota**: Espere a que la implementación se complete. La implementación podría tardar un par de minutos. 

1. Busque y seleccione la máquina virtual `az104-10-vm0`.

1. En la hoja **Copia de seguridad y recuperación ante desastres**, seleccione **Recuperación ante desastres**. 

1. Seleccione **Habilitar replicación**.

1. En la pestaña **Aspectos básicos**, observe la **Región de destino**.

1. Cambie a la pestaña **Configuración avanzada**. Las selecciones de recursos se habrán realizado automáticamente. Es importante revisarlas. 

1. Compruebe la configuración de la suscripción, el grupo de recursos de máquina virtual, la red virtual y la disponibilidad (elija la predeterminada).

1. En **Configuración de almacenamiento**, seleccione **Mostrar detalles**.

    | Configuración | Valor |
    | ---- | ---- |
    | Abandono de la máquina virtual | **Abandono normal**  |
    | Cuenta de almacenamiento en caché | **(nuevo) xxx**  |

   >**Nota:** Es importante que se rellenen ambas opciones de configuración o se producirá un error en la validación. Si los valores no están presentes, pruebe a actualizar la página. Si eso no funciona, cree una cuenta de almacenamiento vacía y vuelva a esta página.

1. En **Configuración de replicación**, seleccione **Mostrar detalles**. Observe que el almacén de recursos de recuperación de la región 2 se ha seleccionado automáticamente.

1. Seleccione **Revisar e iniciar replicación** y, después, **Habilitar replicación**.

    >**Nota**: La habilitación de la replicación tardará entre 10 y 15 minutos en completarse. Vea los mensajes de notificación en la esquina superior derecha del portal. Mientras espera, considere la posibilidad de revisar los vínculos de entrenamiento autodirigido al final de esta página.
    
1. Una vez completada la replicación, localice el almacén de Recovery Services, **az104-rsv-region2**. Es posible que tenga que **actualizar** la página. 

1. En **Elementos protegidos**, seleccione **Elementos replicados**.

1. Compruebe que la máquina virtual se muestre como correcta para el estado de replicación. Tenga en cuenta que el estado mostrará la sincronización (a partir del 0 %) y, por último, mostrará **Protegido** una vez completada la sincronización inicial.

   ![Captura de pantalla de la página de elementos replicados.](../media/az104-lab10-replicated-items.png)

1. Seleccione la máquina virtual para ver más detalles.
   
>**¿Sabía que...?** Se recomienda [probar la conmutación por error de una máquina virtual protegida](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm).

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ ¿Qué productos admite Azure Backup?
+ Resuma los pasos para realizar copias de seguridad y restaurar una máquina virtual de Azure con Azure Backup.
+ Cómo puedo usar Azure PowerShell o la CLI para comprobar el estado de un trabajo de Azure Backup.
+ Proporcione al menos cinco procedimientos recomendados para configurar copias de seguridad de máquinas virtuales de Azure.  

## Más información con el aprendizaje autodirigido

+ [Proteja sus máquinas virtuales con Azure Backup](https://learn.microsoft.com/training/modules/protect-virtual-machines-with-azure-backup/). Use Azure Backup para ayudar a proteger servidores locales, máquinas virtuales, SQL Server, recursos compartidos de archivos de Azure y otras cargas de trabajo.
+ [Protección de la infraestructura de Azure con Azure Site Recovery](https://learn.microsoft.com/en-us/training/modules/protect-infrastructure-with-site-recovery/). Proporcione recuperación ante desastres para la infraestructura de Azure mediante la personalización de la replicación, la conmutación por error y la conmutación por recuperación de máquinas virtuales de Azure con Azure Site Recovery.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ El servicio Azure Backup proporciona soluciones sencillas, seguras y rentables para realizar copias de seguridad y recuperar los datos.
+ Azure Backup puede proteger los recursos locales y en la nube, incluidos los recursos compartidos de archivos y las máquinas virtuales.
+ Las directivas de Azure Backup configuran la frecuencia de las copias de seguridad y el período de retención para los puntos de recuperación. 
+ Azure Site Recovery es una solución de recuperación ante desastres que proporciona protección para las máquinas virtuales y las aplicaciones.
+ Azure Site Recovery replica las cargas de trabajo en un sitio secundario y, en caso de interrupción o desastre, puede conmutar por error al sitio secundario y reanudar las operaciones con un tiempo de inactividad mínimo.
+ Un almacén de Recovery Services almacena los datos de copia de seguridad y minimiza la sobrecarga de administración.
