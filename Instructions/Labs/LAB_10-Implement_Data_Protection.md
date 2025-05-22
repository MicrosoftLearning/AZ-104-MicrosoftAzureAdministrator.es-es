---
lab:
  title: 'Laboratorio 10: Implementación de la protección de datos'
  module: Administer Data Protection
---

# Laboratorio 10: Implementación de la protección de datos

## Introducción al laboratorio    

En este laboratorio, obtendrá información sobre la copia de seguridad y la recuperación de máquinas virtuales de Azure. Aprenderá a crear un almacén de Recovery Service y una directiva de copia de seguridad para máquinas virtuales de Azure. Obtendrá información sobre la recuperación ante desastres con Azure Site Recovery. 

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción podría afectar a la disponibilidad de las características de este laboratorio. Puede cambiar las regiones, pero los pasos se escriben teniendo en cuenta las regiones **Este de EE. UU.** y **Oeste de EE. UU.**

## Tiempo estimado: 50 minutos

## Escenario del laboratorio

Su organización está evaluando cómo realizar copias de seguridad y restaurar máquinas virtuales de Azure a partir de una pérdida de datos accidental o malintencionada. Además, la organización quiere explorar el uso de Azure Site Recovery para escenarios de recuperación ante desastres. 

## Simulación interactiva de laboratorio

Hay una simulación de laboratorio interactiva que puede resultar útil para este tema. La simulación permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure.

+ **[Copia de seguridad de máquinas virtuales y archivos locales.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)**. Cree un almacén de Recovery Services e implemente una copia de seguridad de máquina virtual de Azure. Implemente la copia de seguridad local de carpetas y archivos mediante el agente de Microsoft Azure Recovery Services. Las copias de seguridad locales están fuera del ámbito de este laboratorio, pero puede resultar útil ver esos pasos. 

## Aptitudes de trabajo

+ Tarea 1: Use una plantilla para aprovisionar una infraestructura.
+ Tarea 2: Creación y configuración de un almacén de Recovery Services.
+ Tarea 3: Configure la copia de seguridad de nivel de máquina virtual de Azure.
+ Tarea 4: Supervise Azure Backup.
+ Tarea 5: Habilite la replicación de máquinas virtuales. 

## Tiempo estimado: 40 minutos

## Diagrama de la arquitectura

![Diagrama de las tareas de arquitectura.](../media/az104-lab10-architecture.png)

## Tarea 1: Uso de una plantilla para aprovisionar una infraestructura

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

## Tarea 2: Creación y configuración de un almacén de Recovery Services

En esta tarea, creará un almacén de Recovery Services. Un almacén de Recovery Services proporciona almacenamiento para los datos de la máquina virtual. 

1. En Azure Portal, busca y selecciona `Recovery Services vaults` y, en la hoja **Almacenes de Recovery Services**, haz clic en **+ Crear**.

1. En la hoja **Crear almacén de Recovery Services**, configura las opciones siguientes:

    | Configuración | Valor |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure |
    | Resource group | `az104-rg-region1`  |
    | Nombre del almacén | `az104-rsv-region1` |
    | Region | **Este de EE. UU.** |

    >**Nota**: Asegúrese de especificar la misma región en la que implementó las máquinas virtuales en la tarea anterior.

    ![Captura de pantalla del almacén de Recovery Services.](../media/az104-lab10-create-rsv.png)

1. Haga clic en **Revisar y crear**, asegúrese de que se haya superado la validación y, a continuación, haga clic en **Crear**.

    >**Nota**: espera a que la implementación se complete. La implementación podría tardar un par de minutos. 

1. Cuando se haya completado la implementación, haga clic en **Ir al recurso**.

1. En la sección **Configuración**, haga clic en **Propiedades**.

1. Seleccione el vínculo **Actualizar** en la etiqueta **Configuración de copia de seguridad**.

1. En la hoja **Configuración de copia de seguridad**, revisa las opciones de **Tipo de replicación de almacenamiento**. Deje establecido el valor predeterminado de **Redundancia geográfica** y cierre la hoja.

    >**Nota**: Esta opción solo se puede configurar si no hay elementos de copia de seguridad existentes.
    
    >**¿Sabía que...?** La opción "Restauración entre regiones (CRR)" permite restaurar datos en una [región de Azure emparejada](https://learn.microsoft.com/azure/backup/backup-create-recovery-services-vault#set-cross-region-restore) secundaria. 

1. Seleccione el vínculo **Actualizar** en la etiqueta **Configuración de seguridad > configuración de seguridad y eliminación temporal**.

1. En la hoja **Configuración de seguridad**, tenga en cuenta que la opción **Eliminación temporal (para cargas de trabajo que se ejecutan en Azure)**  está **habilitada**. Fíjese en que el **período de retención de eliminación temporal** es de **14** días. 

>**¿Sabía que...?** Azure tiene dos tipos de almacenes: Almacenes de Recovery Services y almacenes de Backup La principal diferencia reside en los orígenes de datos de los que se puede realizar una copia de seguridad. Obtenga más información sobre [las diferencias](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault).

## Tarea 3: Configuración de la copia de seguridad de nivel de máquina virtual

En esta tarea, implementará la copia de seguridad a nivel de máquina virtual de Azure. Como parte de una copia de seguridad de máquina virtual, deberá definir la directiva de copia de seguridad y retención que se aplica a la copia de seguridad. Las distintas máquinas virtuales pueden tener diferentes directivas de copia de seguridad y retención asignadas.

   >**Nota**: Antes de comenzar esta tarea, asegúrese de que la implementación que inició en la primera tarea de este laboratorio se haya completado correctamente.

1. En la hoja del almacén de Recovery Services, haz clic en **Información general** y, a continuación, haz clic en **+ Copia de seguridad**.

1. En la hoja **Objetivo de Backup**, configura las opciones siguientes:

    | Configuración | Valor |
    | --- | --- |
    | ¿Dónde se ejecuta su carga de trabajo? | **Azure** (vea las otras opciones) |
    | ¿De qué quiere hacer una copia de seguridad? | **Máquina virtual** (vea las otras opciones) |

1. Selecciona **Copia de seguridad**.

1. Observa que hay dos **subtipos de directiva**: **Mejorada** y **Estándar**. Revisa las opciones y selecciona **Estándar**. 

1. En **Directiva de copia de seguridad**, selecciona **Crear una directiva**.

1. Define una nueva directiva de copia de seguridad con las siguientes opciones de configuración (deja las demás con los valores predeterminados):

    | Configuración | Valor |
    | ---- | ---- |
    | Nombre de la directiva | `az104-backup` |
    | Frecuencia | **A diario** |
    | Hora | **12:00 a. m.** |
    | Zona horaria | Nombre de la zona horaria local |
    | Conservar las instantáneas de recuperación instantánea durante | **2** días |

    ![Captura de pantalla de la página de directiva de copia de seguridad.](../media/az104-lab10-backup-policy.png)

1. Haz clic en **Aceptar** para crear la directiva y luego, en la sección **Máquinas virtuales**, selecciona **Añadir** (desplazarte hacia abajo).

1. En la hoja **Seleccionar máquinas virtuales**, selecciona **az-104-10-vm0**, haz clic en **Aceptar** y vuelve a la hoja **Copia de seguridad**; después, haz clic en **Habilitar copia de seguridad**.

    >**Nota**: espera a que se habilite la copia de seguridad. Esta operación debe durar unos 2 minutos.

1. Una vez que la implementación se haya completado, selecciona **Ir al recurso**.
   
1. En la sección **Elementos protegidos**, haz clic en **Elementos de copia de seguridad** y, a continuación, haz clic en la entrada **máquina virtual** de Azure.

1. Selecciona el vínculo **Ver detalles** para **az104-10-vm0** y revisa los valores de las entradas **Comprobación previa de copia de seguridad** y **último estado de copia de seguridad**.

    >**Nota:** fíjate en que la copia de seguridad está pendiente.
    
1. Selecciona **Copia de seguridad ahora**, acepta el valor predeterminado en la lista desplegable **Conservar copia de seguridad hasta** y haz clic en **Aceptar**.

    >**Nota**: no esperes a que se complete la copia de seguridad, sino que avanza a la siguiente tarea.

## Tarea 4: Supervisión de Azure Backup

En esta tarea, implementarás una cuenta de Azure Storage. A continuación, configurarás el almacén para enviar los registros y las métricas a la cuenta de almacenamiento. A continuación, este repositorio se puede usar con Log Analytics u otras soluciones de supervisión de terceros.

1. En Azure Portal, busca y selecciona `Storage accounts`.

1. En la página Cuentas de almacenamiento, selecciona **Crear**.

1. Usa la siguiente información para definir la cuenta de almacenamiento, y después selecciona **Revisar + crear**.

    | Configuración | Valor |
    | --- | --- | 
    | Suscripción          | *Tu suscripción*    |
    | Grupo de recursos        | **az104-rg-region1**        |
    | Nombre de la cuenta de almacenamiento  | Proporciona un nombre único global.   |
    | Región                | **Este de EE. UU.**   |

1. Selecciona **Crear**.

    >**Nota**: espera a que la implementación se complete. Esta operación debería tardar aproximadamente un minuto.

1. Busca y selecciona el almacén de Recovery Services.

1. En la hoja **Supervisión**, selecciona **Configuración de diagnóstico** y después **Agregar configuración de diagnóstico**.

1. Asigna a la configuración el nombre `Logs and Metrics to storage`.

1. Coloca una marca de verificación junto a las siguientes categorías de registro y métricas:

    - **Datos de informes de Azure Backup**
    - **Datos de trabajo de Azure Backup del complemento**
    - **Datos de alerta de Azure Backup del complemento**
    - **Trabajos de Azure Site Recovery**
    - **Eventos de Azure Site Recovery**
    - **Estado**

1. En Detalles de destino, coloca una marca de verificación junto a **Archivar en una cuenta de almacenamiento**.

1. En el campo desplegable Cuenta de almacenamiento, selecciona la cuenta de almacenamiento que implementaste anteriormente en esta tarea.

1. Selecciona **Guardar**.

1. Vuelve al almacén de Recovery Services y, en la hoja **Supervisión**, selecciona **Trabajos de copia de seguridad**.

1. Busca la operación de copia de seguridad de la máquina virtual **az104-10-vm0**. 

1. **Ver los detalles** (desplázate a la derecha para el vínculo) del trabajo de copia de seguridad.

## Tarea 5: Habilitación de la replicación de máquinas virtuales

1. En Azure Portal, busca y selecciona `Recovery Services vaults` y, en la hoja **Almacenes de Recovery Services**, haz clic en **+ Crear**.

1. En la hoja **Crear almacén de Recovery Services**, configura las opciones siguientes:

    | Configuración | Valor |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure |
    | Grupo de recursos | `az104-rg-region2` (Si es necesario, selecciona **Crear nuevo**) |
    | Nombre del almacén | `az104-rsv-region2` |
    | Región | **Oeste de EE. UU.** |

    >**Nota**: asegúrate de especificar una **región diferente** a la máquina virtual.

1. Haz clic en **Revisar y crear**, asegúrate de que se haya superado la validación y, a continuación, haz clic en **Crear**.

    >**Nota**: espera a que la implementación se complete. La implementación podría tardar un par de minutos. 

1. Busca y selecciona la máquina virtual `az104-10-vm0`.

1. En la hoja **Copia de seguridad y recuperación ante desastres**, selecciona **Recuperación ante desastres**. 

1. En la pestaña **Aspectos básicos**, observa la **Región de destino**.

1. Selecciona **Siguiente: Configuración avanzada**. Las selecciones de recursos se han realizado por ti. 

1. Desplázate hacia abajo y **Crea** la cuenta de automatización. 

   >**Nota:** es importante que se rellenen las opciones de configuración o se producirá un error en la validación. 

1. Selecciona **Revisar e iniciar replicación** y, después, **Habilitar replicación**.

    >**Nota**: la habilitación de la replicación tardará entre 10 y 15 minutos en completarse. Visualiza los mensajes de notificación en la esquina superior derecha del portal. Mientras esperas, considera la posibilidad de revisar los vínculos de entrenamiento autodirigido al final de esta página.
    
1. Una vez completada la replicación, localiza el almacén de Recovery Services, **az104-rsv-region2**. Es posible que tenga que **actualizar** la página. 

1. En **Elementos protegidos**, seleccione **Elementos replicados**.

1. Compruebe que la máquina virtual se muestre como correcta para el estado de replicación. Tenga en cuenta que el estado mostrará la sincronización (a partir del 0 %) y, por último, mostrará **Protegido** una vez completada la sincronización inicial.

   ![Captura de pantalla de la página de elementos replicados.](../media/az104-lab10-replicated-items.png)

1. Seleccione la máquina virtual para ver más detalles.
   
>**¿Sabía que...?** Se recomienda [probar la conmutación por error de una máquina virtual protegida](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm).

## Limpieza de los recursos

Si utilizas **tu propia suscripción**, dedica un minuto a eliminar los recursos del laboratorio. De esta forma estarás seguro de que los recursos se liberan y de que se minimiza el coste. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, selecciona el grupo de recursos, selecciona **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haz clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarte a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesitas más información. Abre un explorador Edge y elige Copilot (superior derecha) o ve a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

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
