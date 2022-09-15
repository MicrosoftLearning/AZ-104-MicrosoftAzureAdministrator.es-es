---
lab:
  title: '04: Implementación de redes virtuales'
  module: Administer Virtual Networking
---

# <a name="lab-04---implement-virtual-networking"></a>Laboratorio 04: Implementación de redes virtuales

# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

You need to explore Azure virtual networking capabilities. To start, you plan to create a virtual network in Azure that will host a couple of Azure virtual machines. Since you intend to implement network-based segmentation, you will deploy them into different subnets of the virtual network. You also want to make sure that their private and public IP addresses will not change over time. To comply with Contoso security requirements, you need to protect public endpoints of Azure virtual machines accessible from Internet. Finally, you need to implement DNS name resolution for Azure virtual machines both within the virtual network and from Internet.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Creación y configuración de una red virtual
+ Tarea 2: Implementación de máquinas virtuales en la red virtual
+ Tarea 3: Configuración de direcciones IP privadas y públicas de máquinas virtuales de Azure
+ Tarea 4: Configuración de grupos de seguridad de red
+ Tarea 5: Configuración de Azure DNS para la resolución de nombres internos
+ Tarea 6: Configuración de Azure DNS para la resolución de nombres externos

## <a name="estimated-timing-40-minutes"></a>Tiempo estimado: 40 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab04.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-create-and-configure-a-virtual-network"></a>Tarea 1: Creación y configuración de una red virtual

En esta tarea, creará una red virtual con varias subredes mediante Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En Azure Portal, busque y seleccione **Redes virtuales** y, en la hoja **Redes virtuales**, haga clic en **+ Crear**.

1. Cree una red virtual con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usará en este laboratorio |
    | Grupo de recursos | nombre de un **nuevo** grupo de recursos **az104-04-rg1** |
    | Name | **az104-04-vnet1** |
    | Region | nombre de cualquier región de Azure disponible en la suscripción que usará en este laboratorio |

1. Haga clic en **Siguiente: Direcciones IP** y escriba los siguientes valores

    | Configuración | Value |
    | --- | --- |
    | Espacio de direcciones IPv4 | **10.40.0.0/20** |

1. Haga clic en **+ Agregar subred**, escriba los valores siguientes y, a continuación, haga clic en **Agregar**.

    | Configuración | Value |
    | --- | --- |
    | Nombre de subred | **subnet0** |
    | Intervalo de direcciones de subred | **10.40.0.0/24** |

1. Accept the defaults and click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network to be provisioned. This should take less than a minute.

1. Haga clic en **Ir al recurso**.

1. En la hoja de la red virtual **az104-04-vnet1**, haga clic en **Subredes** y luego en **+ Subred**.

1. Cree una subred con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **subnet1** |
    | Intervalo de direcciones (bloque CIDR) | **10.40.1.0/24** |
    | Grupo de seguridad de red | **None** |
    | Tabla de rutas | **None** |

1. Haga clic en **Guardar**

#### <a name="task-2-deploy-virtual-machines-into-the-virtual-network"></a>Tarea 2: Implementación de máquinas virtuales en la red virtual

En esta tarea, implementará máquinas virtuales de Azure en diferentes subredes de la red virtual mediante una plantilla de ARM.

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**.

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**.

1. En la barra de herramientas del panel de Cloud Shell, haga clic en el icono **Cargar/Descargar archivos**, haga clic en **Cargar** en el menú desplegable y cargue los archivos **\\Allfiles\\Labs\\04\\az104-04-vm-loop-template.json** y **\\Allfiles\\Labs\\04\\az104-04-vm-loop-parameters.json** en el directorio principal de Cloud Shell.

    >**Nota**: Es posible que tenga que cargar cada archivo por separado.

1. Edit the Parameters file, and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. En el panel de Cloud Shell, ejecute lo siguiente para implementar las dos máquinas virtuales con los archivos de parámetros y plantilla:

   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```

    >Debe explorar las funcionalidades de red virtual de Azure.

    >Para empezar, tiene previsto crear una red virtual en Azure que hospedará un par de máquinas virtuales de Azure.

    >**Nota**: Si tiene un error que indica que el tamaño de la máquina virtual no está disponible, pida al instructor ayuda y pruebe estos pasos:
    > 1. Haga clic en el botón `{}` de CloudShell, seleccione **az104-04-vms-loop-parameters.json** en la barra de la izquierda y anote el valor del parámetro `vmSize`.
    > 1. Dado que tiene previsto implementar la segmentación basada en red, las implementará en diferentes subredes de la red virtual.
    > 1. También quiere asegurarse de que sus direcciones IP públicas y privadas no cambiarán con el tiempo.
    > 1. Reemplace el valor del parámetro `vmSize` por uno de los valores devueltos por el comando que acaba de ejecutar.
    > 1. Para cumplir los requisitos de seguridad de Contoso, debe proteger los puntos de conexión públicos de las máquinas virtuales de Azure accesibles desde Internet.

1. Cierre el panel de Cloud Shell.

#### <a name="task-3-configure-private-and-public-ip-addresses-of-azure-vms"></a>Tarea 3: Configuración de direcciones IP privadas y públicas de máquinas virtuales de Azure

En esta tarea, configurará la asignación estática de direcciones IP públicas y privadas asignadas a interfaces de red de máquinas virtuales de Azure.

   >**Nota**: Las direcciones IP privadas y públicas se asignan realmente a las interfaces de red, que, a su vez, están conectadas a máquinas virtuales de Azure; sin embargo, es bastante común hacer referencia a direcciones IP asignadas a máquinas virtuales de Azure en su lugar.

1. En Azure Portal, busque y seleccione **Grupos de recursos** y, en la hoja **Grupos de recursos**, haga clic en **az104-04-rg1**.

1. En la hoja del grupo de recursos **az104-04-rg1**, en la lista de sus recursos, haga clic en **az104-04-vnet1**.

1. En la hoja de la red virtual **az104-04-vnet1**, revise la sección **Dispositivos conectados** y compruebe que hay dos interfaces de red **az104-04-nic0** y **az104-04-nic1** conectadas a la red virtual.

1. Haga clic en **az104-04-nic0** y, en la hoja de **az104-04-nic0**, haga clic en **Configuraciones de IP**.

    >**Nota**: Compruebe que **ipconfig1** está configurado actualmente con una dirección IP privada dinámica.

1. En la lista de configuraciones de IP, haga clic en **ipconfig1**.

1. En la hoja de **ipconfig1**, en la sección **Configuración de dirección IP pública**, seleccione **Asociar**, haga clic en **+ Crear nuevo**, especifique los siguientes valores y haga clic en **Aceptar**:

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **az104-04-pip0** |
    | SKU | **Estándar** |

1. En la hoja de **ipconfig1**, establezca **Asignación** en **Estática**, y deje el valor predeterminado de **Dirección IP** establecido en **10.40.0.4**.

1. Por último, debe implementar la resolución de nombres DNS para las máquinas virtuales de Azure tanto dentro de la red virtual como desde Internet.

1. Vuelva a la hoja de **az104-04-vnet1**.

1. Haga clic en **az104-04-nic1** y, en la hoja de **az104-04-nic1**, haga clic en **Configuraciones de IP**.

    >**Nota**: Compruebe que **ipconfig1** está configurado actualmente con una dirección IP privada dinámica.

1. En la lista de configuraciones de IP, haga clic en **ipconfig1**.

1. En la hoja de **ipconfig1**, en la sección **Configuración de dirección IP pública**, seleccione **Asociar**, haga clic en **+ Crear nuevo**, especifique los siguientes valores y haga clic en **Aceptar**:

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **az104-04-pip1** |
    | SKU | **Estándar** |

1. En la hoja de **ipconfig1**, establezca **Asignación** en **Estática**, y deje el valor predeterminado de **Dirección IP** establecido en **10.40.1.4**.

1. De nuevo en la hoja de **ipconfig1**, guarde los cambios.

1. Vuelva a la hoja del grupo de recursos **az104-04-rg1**, en la lista de sus recursos, haga clic en **az104-04-vm0** y, en la hoja de la máquina virtual **az104-04-vm0**, anote la entrada de la dirección IP pública.

1. Vuelva a la hoja del grupo de recursos **az104-04-rg1**, en la lista de sus recursos, haga clic en **az104-04-vm1** y, en la hoja de la máquina virtual **az104-04-vm1**, anote la entrada de la dirección IP pública.

    >**Nota**: Necesitará ambas direcciones IP en la última tarea de este laboratorio.

#### <a name="task-4-configure-network-security-groups"></a>Tarea 4: Configuración de grupos de seguridad de red

En esta tarea, configurará grupos de seguridad de red para permitir la conectividad restringida a las máquinas virtuales de Azure.

1. En Azure Portal, vuelva a la hoja del grupo de recursos **az104-04-rg1** y, en la lista de sus recursos, haga clic en **az104-04-vm0**.

1. En la hoja de información general de **az104-04-vm0**, haga clic en **Conectar**, en el menú desplegable, haga clic en **RDP**, en la hoja **Conectar con RDP**, haga clic en **Descargar archivo RDP** usando la dirección IP pública y siga las indicaciones para iniciar la sesión de Escritorio remoto.

1. Observe que se produce un error en el intento de conexión.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is expected, because public IP addresses of the Standard SKU, by default, require that the network interfaces to which they are assigned are protected by a network security group. In order to allow Remote Desktop connections, you will create a network security group explicitly allowing inbound RDP traffic from Internet and assign it to network interfaces of both virtual machines.

1. Detenga las máquinas virtuales **az104-04-vm0** y **az104-04-vm1**.

    >                **Nota:** Hay disponible una **[simulación de laboratorio interactiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)** que le permite realizar sus propias selecciones a su entera discreción.

1. En Azure Portal, busque y seleccione **Grupos de seguridad de red** y, en la hoja **Grupos de seguridad de red**, haga clic en **+ Crear**.

1. Cree un grupo de seguridad de red con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | **az104-04-rg1** |
    | Name | **az104-04-nsg01** |
    | Region | nombre de la región de Azure en la que implementó todos los demás recursos de este laboratorio |

1. Es posible que encuentre pequeñas diferencias entre la simulación interactiva y el laboratorio hospedado, pero las ideas y los conceptos básicos que se muestran son los mismos.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 2 minutes.

1. En la hoja de implementación, haga clic en **Ir al recurso** para abrir la hoja del grupo de seguridad de red **az104-04-nsg01**.

1. En la hoja del grupo de seguridad de red **az104-04-nsg01**, en la sección **Configuración**, haga clic en **Reglas de seguridad de entrada**.

1. Agregue una regla de entrada con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Source | **Cualquiera** |
    | Source port ranges | * |
    | Destination | **Cualquiera** |
    | Servicio | **RDP** |
    | Acción | **Permitir** |
    | Priority | **300** |
    | Name | **AllowRDPInBound** |

1. En la hoja del grupo de seguridad de red **az104-04-nsg01**, en la sección **Configuración**, haga clic en **Interfaces de red** y luego en **+ Asociar**.

1. Asocie el grupo de seguridad de red **az104-04-nsg01** a las interfaces de red **az104-04-nic0** y **az104-04-nic1**.

    >**Nota**: Las reglas del grupo de seguridad de red recién creado pueden tardar hasta 5 minutos en aplicarse a la tarjeta de interfaz de red.

1. Inicie las máquinas virtuales **az104-04-vm0** y **az104-04-vm1**.

1. Vuelva a la hoja de la máquina virtual **az104-04-vm0**.

    >**Nota:** En los pasos siguientes, comprobará que puede conectarse correctamente a la máquina virtual de destino.

1. En la hoja de **az104-04-vm0**, haga clic en **Conectar**, haga clic en **RDP**, en la hoja **Conectar con RDP**, haga clic en **Descargar archivo RDP** usando la dirección IP pública y siga las indicaciones para iniciar la sesión de Escritorio remoto.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**Nota**: Puede omitir cualquier aviso de advertencia al conectarse a las máquinas virtuales de destino.

1. Cuando el sistema se lo indique, inicie sesión con el usuario y la contraseña del archivo de parámetros.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Leave the Remote Desktop session open. You will need it in the next task.

#### <a name="task-5-configure-azure-dns-for-internal-name-resolution"></a>Tarea 5: Configuración de Azure DNS para la resolución de nombres internos

En esta tarea, configurará la resolución de nombres DNS dentro de una red virtual mediante zonas DNS privadas de Azure.

1. En Azure Portal, busque y seleccione **Zonas DNS privadas** y, en la hoja **Zonas DNS privadas**, haga clic en **+ Crear**.

1. Cree una zona DNS privada con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | **az104-04-rg1** |
    | Name | **contoso.org** |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. Haga clic en **Ir al recurso** para abrir la hoja de zona DNS privada de **contoso.org**.

1. En la hoja de zona DNS privada de **contoso.org**, en la sección **Configuración**, haga clic en **Vínculos de la red virtual**.

1. Haga clic en **+ Agregar** para crear una red virtual con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre del vínculo | **az104-04-vnet1-link** |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Virtual network | **az104-04-vnet1** |
    | Habilitación del registro automático | enabled |

1. Haga clic en **Aceptar**.

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network link to be created. This should take less than 1 minute.

1. En la hoja de zona DNS privada de **contoso.org**, en la barra lateral, haga clic en **Descripción general**.

1. Compruebe que los registros DNS de **az104-04-vm0** y **az104-04-vm1** aparecen en la lista de conjuntos de registros como **Registrado automáticamente**.

    >**Nota**: Es posible que tenga que esperar unos minutos y actualizar la página si los conjuntos de registros no aparecen en la lista.

1. Cambie a la sesión de Escritorio remoto para **az104-04-vm0**, haga clic con el botón derecho en el botón **Inicio** y, en el menú contextual, haga clic en **Windows PowerShell (Administrador)** .

1. En la ventana de la consola Windows PowerShell, ejecute lo siguiente para probar la resolución interna de nombres en la zona DNS privada recién creada:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Compruebe que la salida del comando incluye la dirección IP privada de **az104-04-vm1** (**10.40.1.4**).

#### <a name="task-6-configure-azure-dns-for-external-name-resolution"></a>Tarea 6: Configuración de Azure DNS para la resolución de nombres externos

En esta tarea, configurará la resolución de nombres DNS externos mediante zonas DNS públicas de Azure.

1. En un explorador web, abra una nueva pestaña y vaya a <https://www.godaddy.com/domains/domain-name-search>.

1. Use la búsqueda de nombres de dominio para identificar un nombre de dominio que no esté en uso.

1. En Azure Portal, busque y seleccione **Zonas DNS** y, en la hoja **Zonas DNS**, haga clic en **+ Crear**.

1. Cree una zona DNS con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Grupo de recursos | **az104-04-rg1** |
    | Name | nombre de dominio DNS que identificó anteriormente en esta tarea |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the DNS zone to be created. This should take about 2 minutes.

1. Haga clic en **Ir al recurso** para abrir la hoja de la zona DNS recién creada.

1. En la hoja de zona DNS, haga clic en **+ Conjunto de registros**.

1. Agregue un conjunto de registros con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **az104-04-vm0** |
    | Tipo | **A** |
    | Conjunto de registros de alias | **No** |
    | TTL | **1** |
    | Unidad de TTL | **Horas** |
    | Dirección IP | dirección IP pública de **az104-04-vm0** que identificó en el tercer ejercicio de este laboratorio |

1. Haga clic en **Aceptar**

1. En la hoja de zona DNS, haga clic en **+ Conjunto de registros**.

1. Agregue un conjunto de registros con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **az104-04-vm1** |
    | Tipo | **A** |
    | Conjunto de registros de alias | **No** |
    | TTL | **1** |
    | Unidad de TTL | **Horas** |
    | Dirección IP | dirección IP pública de **az104-04-vm1** que identificó en el tercer ejercicio de este laboratorio |

1. Haga clic en **Aceptar**

1. En la hoja de zona DNS, anote el nombre de la entrada **Servidor DNS 1**.

1. En Azure Portal, abra la sesión de **PowerShell** en **Cloud Shell**. Para ello, haga clic en el icono de la esquina superior derecha de Azure Portal.

1. En el panel Cloud Shell, ejecute lo siguiente para probar la resolución de nombres externos del conjunto de registros DNS de **az104-04-vm0** en la zona DNS recién creada (reemplace el marcador de posición `[Name server 1]` por el nombre de **Servidor DNS 1** que anotó anteriormente en esta tarea y el marcador de posición `[domain name]` por el nombre del dominio DNS que creó anteriormente en esta tarea):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Compruebe que la salida del comando incluye la dirección IP pública de **az104-04-vm0**.

1. En el panel Cloud Shell, ejecute lo siguiente para probar la resolución de nombres externos del conjunto de registros DNS de **az104-04-vm1** en la zona DNS recién creada (reemplace el marcador de posición `[Name server 1]` por el nombre de **Servidor DNS 1** que anotó anteriormente en esta tarea y el marcador de posición `[domain name]` por el nombre del dominio DNS que creó anteriormente en esta tarea):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Compruebe que la salida del comando incluye la dirección IP pública de **az104-04-vm1**.

#### <a name="clean-up-resources"></a>Limpieza de recursos

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. En Azure Portal, abra la sesión de **PowerShell** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro -AsJob). Aunque podrá ejecutar otro comando de PowerShell inmediatamente después en la misma sesión de PowerShell, los grupos de recursos tardarán unos minutos en eliminarse.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

+ Creado y configurado una red virtual
+ Implementado máquinas virtuales en la red virtual
+ Configurado direcciones IP privadas y públicas de máquinas virtuales de Azure
+ Configurado grupos de seguridad de red
+ Configurado Azure DNS para la resolución de nombres internos
+ Configurado Azure DNS para la resolución de nombres externos
