---
lab:
  title: "Laboratorio\_08: Administración de máquinas virtuales"
  module: Administer Virtual Machines
---

# Laboratorio 08: Administración de máquinas virtuales

## Introducción al laboratorio

En este laboratorio, creará máquinas virtuales y las comparará con conjuntos de escalado de máquinas virtuales. Descubrirá cómo crear, configurar y cambiar el tamaño de una máquina virtual única. Descubrirá cómo crear un conjunto de escalado de máquinas virtuales y configurar el escalado automático.

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 50 minutos

## Escenario del laboratorio

Su organización quiere explorar la implementación y configuración de máquinas virtuales de Azure. En primer lugar, implementará una máquina virtual de Azure con escalado manual. A continuación, implementará un conjunto de escalado de máquinas virtuales y explorará el escalado automático.

## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure.

+ [Crear una máquina virtual en el portal](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%201). Cree una máquina virtual, conéctela e instale el rol de servidor web.

+ [Implementar una máquina virtual con una plantilla](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Explore la galería de inicio rápido y localice una plantilla de máquina virtual. Implementar la plantilla y comprobar el resultado.

+ [Crear una máquina virtual con PowerShell](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2010). Use Azure PowerShell para implementar una máquina virtual. Revise las recomendaciones de Azure Advisor.

+ [Crear una máquina virtual con la CLI](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2011). Use la CLI para implementar una máquina virtual. Revise las recomendaciones de Azure Advisor.

## Aptitudes de trabajo

+ Tarea 1: Implementación de máquinas virtuales de Azure con resistencia de zona mediante Azure Portal.
+ Tarea 2: Administración del escalado del proceso y el almacenamiento para máquinas virtuales.
+ Tarea 3: Creación y configuración de conjuntos de escalado de máquinas virtuales de Azure.
+ Tarea 4: Escalado de los Conjuntos de escalado de las máquinas virtuales de Azure.
+ Tarea 5: Creación de una máquina virtual mediante Azure PowerShell (opcional 1).
+ Tarea 6: Creación de una máquina virtual con la CLI (opcional 2).

## Diagrama de arquitectura de Azure Virtual Machines

![Diagrama de las tareas de arquitectura de VM.](../media/az104-lab08-vm-architecture.png)

## Tarea 1: Implementación de máquinas virtuales de Azure con resistencia de zona mediante Azure Portal

En esta tarea, implementará dos máquinas virtuales de Azure en diferentes zonas de disponibilidad mediante Azure Portal. Las zonas de disponibilidad ofrecen el nivel más alto de Acuerdo de Nivel de Servicio de tiempo de actividad para las máquinas virtuales, un 99,99 %. Para lograr este Acuerdo de Nivel de Servicio, debe implementar al menos dos máquinas virtuales en distintas zonas de disponibilidad.

1. Inicie sesión en Azure Portal: `https://portal.azure.com`.

1. Busque y seleccione `Virtual machines`, en la hoja **Máquinas virtuales**, haga clic en **+ Crear** y, después, seleccione en la lista desplegable **Máquina virtual de Azure**. Observe las otras opciones.

1. En la pestaña **Aspectos básicos**, en el menú desplegable **Zona de disponibilidad**, coloque una marca de verificación junto a **Zona 2**. Esto debe seleccionar tanto la **Zona 1** como la **Zona 2**.

    >**Nota**: Esto implementará dos máquinas virtuales en la región seleccionada, una en cada zona. El Acuerdo de Nivel de Servicio de tiempo de actividad del 99,99 % se alcanza porque tiene al menos dos máquinas virtuales distribuidas entre al menos dos zonas. En el escenario en el que puede que solo necesite una máquina virtual, seguir implementando la máquina virtual en otra zona es un procedimiento recomendado.

1. En la pestaña Aspectos básicos, continúe completando la configuración:

    | Configuración | Valor |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure |
    | Resource group |  **az104-rg8** (si es necesario, haga clic en **Crear nuevo**). |
    | Nombres de las máquinas virtuales | `az104-vm1` y `az104-vm2` (Después de seleccionar ambas zonas de disponibilidad, seleccione **Editar nombres** en el campo Nombre de la máquina virtual). |
    | Region | **Este de EE. UU.** |
    | Opciones de disponibilidad | **Zona de disponibilidad** |
    | Zona de disponibilidad | **Zona 1, 2** (lea la nota sobre el uso de conjuntos de escalado de máquinas virtuales). |
    | Tipo de seguridad | **Estándar** |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Instancia de Azure Spot | **unchecked** |
    | Size | **Estándar D2s v3** |
    | Nombre de usuario | `localadmin` |
    | Contraseña | **Proporcione una contraseña segura** |
    | Puertos de entrada públicos | **None** |
    | ¿Quiere usar una licencia de Windows Server existente? | **Desactivado** |

    ![Captura de pantalla de la página Crear máquina virtual.](../media/az104-lab08-create-vm.png)

1. Haga clic en **Siguiente: Discos >**, especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Tipo de disco del sistema operativo | **SSD Premium** |
    | Eliminar con VM | **activado** (valor predeterminado) |
    | Habilitar compatibilidad con Disco Ultra | **Desactivado** |

1. Haga clic en **Siguiente: Redes >** toman los valores predeterminados, pero no proporcionan un equilibrador de carga.

    | Configuración | Valor |
    | --- | --- |
    | Eliminación de la IP pública y NIC cuando se elimine la VM | **Activada** |
    | Opciones de equilibrio de carga | **None** |


1. Haga clic en **Siguiente: Administración >** y especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Opciones de orquestación de revisiones | **Orquestado por Azure** |  

1. Haga clic en **Siguiente: Supervisión >** y especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Diagnósticos de arranque | **Deshabilitar** |

1. Haga clic en **Siguiente: Avanzado >**, tome los valores predeterminados y haga clic en **Revisar y crear**.

1. Después de la validación, haga clic en **Crear**.

    >**Nota:** Observe que la máquina virtual implementa la NIC, el disco y la IP pública (si están configuradas) se crean y administran de forma independiente.

1. Espere a que se complete la implementación y seleccione **Ir al recurso**.

   >**Nota:** Supervise los mensajes de **Notificación**.

## Tarea 2: Administración del escalado del proceso y el almacenamiento para máquinas virtuales

En esta tarea, escalará una máquina virtual ajustando su tamaño a una SKU diferente. Azure proporciona flexibilidad en la selección de tamaño de máquina virtual para que pueda ajustar una máquina virtual durante periodos si necesita más (o menos) proceso y memoria asignadas. Este concepto se extiende a los discos, donde puede modificar el rendimiento del disco o aumentar la capacidad asignada.

1. En la máquina virtual **az104-vm1**, en la hoja **Disponibilidad y escala**, seleccione **Tamaño**.

1. Establezca el tamaño de la máquina virtual en **DS1_v2** y haga clic en **Cambiar tamaño**. Cuando se le solicite, confirme el cambio.

    >**Nota**: Elija otro tamaño si **Estándar DS1_v2** no está disponible. El cambio de tamaño también se conoce como escalado vertical o reducción vertical.

    ![Captura de pantalla del cambio de tamaño de la máquina virtual.](../media/az104-lab08-resize-vm.png)

1. En el área **Configuración**, seleccione **Discos**.

1. En **Discos de datos**, seleccione **+ Crear y adjuntar un nuevo disco**. Configure los ajustes (deje las otras opciones de configuración con sus valores predeterminados).

    | Configuración | Value |
    | --- | --- |
    | Nombre del disco | `vm1-disk1` |
    | Tipo de almacenamiento | **HDD estándar** |
    | Tamaño (GiB) | `32` |

1. Haga clic en **Aplicar**.

1. Una vez creado el disco, haga clic en **Desasociar** (si es necesario, desplácese a la derecha para ver el icono de desasociación) y luego en **Aplicar**.

    >**Nota**: La desasociación quita el disco de la máquina virtual, pero lo sigue almacenando para su uso posterior.

1. Busque y seleccione `Disks`. En la lista de discos, seleccione el objeto **vm1-disk1**.

    >**Nota:** La hoja **Información general** también proporciona información de rendimiento y uso para el disco.

1. En la hoja **Configuración**, seleccione **Tamaño y rendimiento**.

1. Establezca el tipo de almacenamiento en **SSD estándar** y, luego, haga clic en **Guardar**.

1. Vuelva a la máquina virtual **az104-vm1** y seleccione **Discos**.

1. En la sección **Disco de datos**, seleccione **Adjuntar discos existentes**.

1. En la lista desplegable **Nombre de disco**, seleccione **VM1-DISK1**. 

1. Compruebe que el disco ahora es **SSD estándar**.

1. Seleccione **Aplicar** para guardar los cambios. 

    >**Nota:** Ahora ha creado una máquina virtual, ha escalado la SKU y el tamaño del disco de datos. En la siguiente tarea se usan conjuntos de escalado de máquinas virtuales para automatizar el proceso de escalado.

## Diagrama de arquitectura de Conjuntos de escalado de máquinas virtuales de Azure

![Diagrama de las tareas de arquitectura de VMSS.](../media/az104-lab08-vmss-architecture.png)

## Tarea 3: Creación y configuración de conjuntos de escalado de máquinas virtuales de Azure

En esta tarea, implementará un conjunto de escalado de máquinas virtuales de Azure entre zonas de disponibilidad. Los conjuntos de escalado de máquinas virtuales reducen la sobrecarga administrativa de la automatización al permitirle configurar métricas o condiciones que permitan que el conjunto de escalado se escale horizontalmente o se reduzca horizontalmente.

1. En Azure Portal, busque y seleccione `Virtual machine scale sets` y, en la hoja **Conjuntos de escalado de máquinas virtuales**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear un conjunto de escalado de máquinas virtuales**, especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados) y haga clic en **Siguiente: Spot >**.

    | Configuración | Valor |
    | --- | --- |
    | Suscripción | el nombre de la suscripción de Azure  |
    | Resource group | **az104-rg8**  |
    | Nombre del conjunto de escalado de máquinas virtuales | `vmss1` |
    | Region | **(EE. UU.) Este de EE. UU.** |
    | Zona de disponibilidad | **Zonas 1, 2, 3** |
    | Modo de orquestación | **Uniforme** |
    | Tipo de seguridad | **Estándar** |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Ejecución de Azure Spot con descuento | **Desactivado** |
    | Size | **Estándar D2s_v3** |
    | Nombre de usuario | `localadmin` |
    | Contraseña | **Proporcione una contraseña segura**  |
    | ¿Ya tiene una licencia de Windows Server? | **Desactivado** |

    >**Nota**: Para obtener la lista de regiones de Azure que admiten la implementación de máquinas virtuales de Windows en zonas de disponibilidad, consulte [¿Qué son las zonas de disponibilidad en Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![Captura de pantalla de la página Crear VMSS. ](../media/az104-lab08-create-vmss.png)

1. En la pestaña **Máquina virtual de acceso puntual**, acepte los valores predeterminados y seleccione **Siguiente: Discos >**.

1. En la pestaña **Discos**, acepte los valores predeterminados y haga clic en **Siguiente: Redes >**.

1. En la página **Redes**, haga clic en el vínculo **Crear red virtual** debajo del cuadro de texto **Red virtual** y cree una red virtual con las siguientes opciones de configuración (deje las demás con sus valores predeterminados).  Cuando termine, seleccione **Aceptar**.

    | Configuración | Value |
    | --- | --- |
    | Nombre | `vmss-vnet` |
    | Intervalo de direcciones | `10.82.0.0/20` (cambie lo que hay ahí). |
    | Nombre de subred | `subnet0` |
    | Rango de subred | `10.82.0.0/24` |

1. En la pestaña **Redes**, haga clic en el icono **Editar interfaz de red** a la derecha de la entrada de la interfaz de red.

1. En la sección **Grupo de seguridad de red NIC**, seleccione **Avanzado** y, después, haga clic en **Crear nuevo** en la lista desplegable **Configurar grupo de seguridad de red**.

1. En la hoja **Crear grupo de seguridad de red**, especifique las opciones de configuración siguientes (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre | **vmss1-nsg** |

1. Haga clic en **Agregar una regla de entrada** y agregue una regla de seguridad de entrada con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Origen | **Cualquiera** |
    | Rangos del puerto origen | * |
    | Destino | **Cualquiera** |
    | Servicio | **HTTP** |
    | Action | **Permitir** |
    | Priority | **1010** |
    | Nombre | `allow-http` |

1. Haga clic en **Agregar** y, de nuevo en la hoja **Crear grupo de seguridad de red**, haga clic en **Aceptar**.

1. En la hoja **Editar la interfaz de red**, en la sección **Dirección IP pública**, haga clic en **Habilitada** y en **Aceptar**.

1. En la pestaña **Redes**, en la sección **Equilibrio de carga**, especifique lo siguiente (deje a otros con sus valores predeterminados).

    | Configuración | Valor |
    | --- | --- |
    | Opciones de equilibrio de carga | **Azure Load Balancer** |
    | Seleccionar un equilibrador de carga | **Creación de un equilibrador de carga** |

1. En la página **Crear un equilibrador de carga**, especifique el nombre del equilibrador de carga y tome los valores predeterminados. Haga clic en **Crear** cuando haya terminado **Siguiente: Administración >**.

    | Configuración | Valor |
    | --- | --- |
    | Nombre del equilibrador de carga | `vmss-lb` |

    >**Nota:** Deténgase un minuto y revise lo que ha hecho. En este momento, ha configurado el conjunto de escalado de máquinas virtuales con discos y redes. En la configuración de red, ha creado un grupo de seguridad de red y ha permitido HTTP. También ha creado un equilibrador de carga con una IP pública.

1. En la pestaña **Administración**, especifique las opciones de configuración siguientes (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Diagnósticos de arranque | **Deshabilitar** |

1. Haga clic en **Siguiente: Estado >**.

1. En la pestaña **Estado**, revise la configuración predeterminada sin realizar ningún cambio y haga clic en **Siguiente: Opciones avanzadas >**.

1. En la pestaña **Opciones avanzadas**, haga clic en **Revisar y crear**.

1. En la pestaña **Revisar y crear**, asegúrese de que se haya superado la validación y haga clic en **Crear**.

    >**Nota**: Espere a que se complete la implementación del conjunto de escalado de máquinas virtuales. Esto debe durar unos 5 minutos. Mientras espera, revise la [documentación](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview).

## Tarea 4: Escalado de los Conjuntos de escalado de las máquinas virtuales de Azure

En esta tarea, se escala el conjunto de escalado de máquinas virtuales mediante una regla de escalado personalizada.

1. Seleccione **Ir al recurso** o busque y seleccione el conjunto de escalado **vmss1**.

1. Elija **Disponibilidad y escalado** en el menú de la izquierda y, a continuación, elija **Escalado**.

>**¿Sabía que...?** Puede realizar una **Escala manual** o una **Escalabilidad automática personalizada**. En conjuntos de escalado con un pequeño número de instancias de máquina virtual, puede ser mejor opción aumentar o reducir el número de instancias (escala manual). En conjuntos de escalado con un gran número de instancias de máquina virtual, el escalado basado en métricas (escalabilidad automática personalizada) puede ser más adecuado.

### Regla de escalabilidad horizontal

1. Seleccione **Escalabilidad automática personalizada**. Después, cambie el **Modo de escalado** a **Escala basada en métricas**. A continuación, seleccione **Agregar una regla**.

1. Vamos a crear una regla que aumente automáticamente el número de instancias de máquina virtual. Esta regla se escala horizontalmente cuando la carga media de la CPU es superior al 70 % durante un periodo de 10 minutos. Cuando se desencadena la regla, aumenta el número de instancias de máquina virtual en un 20 %.

    | Configuración | Valor |
    | --- | --- |
    | Origen de métricas | **Recurso actual (vmss1)** |
    | Espacio de nombres de métricas | **Host de máquina virtual** |
    | Nombre de métrica | **Porcentaje de CPU** (revisar las otras opciones) |
    | Operador | **Mayor que** |
    | Umbral de la métrica para desencadenar la acción de escalado | **70** |
    | Duración (minutos) | **10**
           |
    | Estadísticas de intervalo de agregación | **Average** |
    | Operación | **Aumentar porcentaje por** (revisar otras opciones) |
    | Tiempo de finalización (minutos) | **5** |
    | Percentage | **20** |

    ![Captura de pantalla de la página Agregar regla de escalado.](../media/az104-lab08-scale-rule.png)

1. Asegúrese de **Guardar** los cambios.

### Regla de reducción horizontal

1. Durante las noches o fines de semana, la demanda puede disminuir, por lo que es importante crear una regla de reducción horizontal.

1. Vamos a crear una regla que reduzca el número de instancias de máquina virtual en un conjunto de escalado. El número de instancias debe disminuir cuando la carga media de la CPU disminuya por debajo del 30 % durante un periodo de 10 minutos. Cuando se desencadena la regla, reduzca el número de instancias de máquina virtual en un 20 %.

1. Seleccione **Agregar una regla**, ajuste la configuración y, después, seleccione **Agregar**.

    | Configuración | Valor |
    | --- | --- |
    | Operador | **Menor que** |
    | Umbral | **30** |
    | Operación | **Disminuir porcentaje por** (revisar las otras opciones) |
    | Percentage | **20** |

1. Asegúrese de **Guardar** los cambios.

### Establecimiento de los límites de la instancia

1. Cuando se aplican las reglas de escalado automático, los límites de instancia garantizan que no se realiza un escalado horizontal que supere el número máximo de instancias ni se realiza una reducción horizontal que no llegue al número mínimo de instancias.

1. Los **límites de instancia** se muestran en la página **Escalado** detrás de las reglas.

    | Configuración | Valor |
    | --- | --- |
    | Mínima | **2** |
    | Máximo | **10**
           |
    | Valor predeterminado | **2** |

1. Asegúrese de **guardar** los cambios.

1. En la página**vmss1**, seleccione **Instancias**. Aquí es donde supervisaría el número de instancias de máquina virtual.

    >**Nota:** Si está interesado en usar Azure PowerShell para la creación de máquinas virtuales, pruebe la tarea 5. Si está interesado en usar la CLI para crear máquinas virtuales, pruebe la tarea 6.

## Tarea 5: Creación de una máquina virtual mediante Azure PowerShell (opcional 1)

1. Use el icono (superior derecho) para iniciar una sesión de **Cloud Shell**. Como alternativa, vaya directamente a `https://shell.azure.com`.

1. Asegúrese de seleccionar **PowerShell**. Si es necesario, configure el almacenamiento del shell.

1. Ejecute el comando siguiente para crear una máquina virtual. Cuando se le solicite, proporcione un nombre de usuario y una contraseña para la máquina virtual. Mientras espera, consulte la referencia del comando [New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) para todos los parámetros asociados a la creación de una máquina virtual.

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' `
    -Credential (Get-Credential)
    ```

1. Una vez completado el comando, use **Get-AzVM** para mostrar una lista de las máquinas virtuales del grupo de recursos.

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8' `
    -Status
    ```

1. Compruebe que aparece la nueva máquina virtual y que el **Estado** es **En ejecución**.

1. Use **Stop-AzVM** para desasignar la máquina virtual. Escriba **Sí** para confirmar

    ```powershell
    Stop-AzVM `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' 
    ```

1. Use **Get-AzVM** con el parámetro **-Status** para comprobar que la máquina está **desasignada**.

    >**¿Sabía que...?** Cuando se usa Azure para detener la máquina virtual, el estado se *desasigna*. Esto significa que se liberan las IP públicas no estáticas y se dejan de pagar los costos de proceso de la máquina virtual.

## Tarea 6: Creación de una máquina virtual con la CLI (opcional 2)

1. Use el icono (superior derecho) para iniciar una sesión de **Cloud Shell**. Como alternativa, vaya directamente a `https://shell.azure.com`.

1. Asegúrese de seleccionar **Bash**. Si es necesario, configure el almacenamiento del shell.

1. Ejecute el comando siguiente para crear una máquina virtual. Cuando se le solicite, proporcione un nombre de usuario y una contraseña para la máquina virtual. Mientras espera, consulte la referencia del comando [az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) para todos los parámetros asociados a la creación de una máquina virtual.

    ```sh
    az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys
    ```

1. Una vez completado el comando, use **az vm show** para comprobar que se creó la máquina.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-rg8 --show-details
    ```

1. Compruebe que **powerState** es **VM en ejecución**.

1. Use **az vm deallocate** para desasignar la máquina virtual. Escriba **Sí** para confirmar

    ```sh
    az vm deallocate --resource-group az104-rg8 --name myCLIVM
    ```

1. Use **az vm show** para asegurarse de que **powerState** es **Máquina virtual desasignada**.

    >**¿Sabía que...?** Cuando se usa Azure para detener la máquina virtual, el estado se *desasigna*. Esto significa que se liberan las IP públicas no estáticas y se dejan de pagar los costos de proceso de la máquina virtual.

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ Proporcione los pasos y los comandos de la CLI de Azure para crear una máquina virtual Linux. 
+ Revise las formas en que puede escalar máquinas virtuales y mejorar el rendimiento.
+ Describir las directivas de administración del ciclo de vida de Azure Storage y cómo pueden optimizar los costos.

## Más información con el aprendizaje autodirigido

+ [Crear una máquina virtual Windows en Azure](https://learn.microsoft.com/training/modules/create-windows-virtual-machine-in-azure/). Crear una máquina virtual de Windows mediante Azure Portal. Conectarse a una máquina virtual Windows mediante Escritorio remoto
+ [Compilar una aplicación escalable con conjuntos de escalado de máquinas virtuales](https://learn.microsoft.com/training/modules/build-app-with-scale-sets/). Permita que la aplicación ajuste automáticamente los cambios en la carga, a la vez que minimiza los costos con Virtual Machine Scale Sets.
+ [Conéctese a máquinas virtuales desde Azure Portal con Azure Bastion](https://learn.microsoft.com/en-us/training/modules/connect-vm-with-azure-bastion/). Implemente Azure Bastion para conectarse de forma segura a máquinas virtuales de Azure directamente en Azure Portal para reemplazar eficazmente una solución de jumpbox existente, supervise las sesiones remotas mediante registros de diagnóstico y administre sesiones remotas mediante la desconexión de una sesión de usuario.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio.

+ Azure Virtual Machines son recursos informáticos escalables a petición.
+ Las máquinas virtuales de Azure proporcionan opciones de escalado vertical y horizontal.
+ La configuración de máquinas virtuales de Azure incluye la selección de un sistema operativo, un tamaño, un almacenamiento y una configuración de red.
+ Los conjuntos de escalado de máquinas virtuales de Azure permiten crear y administrar un grupo de máquinas virtuales con equilibrio de carga.
+ Las máquinas virtuales de un conjunto de escalado de máquinas virtuales se crean a partir de la misma imagen y configuración.
+ En un conjunto de escalado de máquinas virtuales, el número de instancias de máquina virtual puede aumentar o disminuir automáticamente según la demanda, o en respuesta a una programación definida.
