---
lab:
  title: '06: Implementación de la administración del tráfico'
  module: Module 06 - Network Traffic Management
ms.openlocfilehash: 45507e4f181e339eaceae6cce9b6a8da84627a45
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625620"
---
# <a name="lab-06---implement-traffic-management"></a>Laboratorio 06: Implementación de la administración del tráfico
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

Se le ha encargado probar la administración del tráfico de red dirigido a máquinas virtuales de Azure en la topología de red en estrella tipo hub-and-spoke, que Contoso está considerando implementar en su entorno de Azure (en lugar de crear la topología de malla, que probó en el laboratorio anterior). Estas pruebas deben incluir la implementación de conectividad entre radios mediante rutas definidas por el usuario que fuercen el flujo de tráfico a través del centro, así como la distribución del tráfico entre máquinas virtuales mediante equilibradores de carga de nivel 4 y nivel 7. Para ello, tiene previsto usar Azure Load Balancer (nivel 4) y Azure Application Gateway (nivel 7).

>**Nota**: Este laboratorio, de manera predeterminada, requiere un total de 8 vCPU disponibles en la serie Standard_Dsv3 en la región que elija para la implementación, ya que implica la implementación de cuatro VM de Azure de SKU Standard_D2s_v3. Si los alumnos usan cuentas de prueba, con el límite de 4 vCPU, puede usar un tamaño de VM que requiera solo una vCPU (por ejemplo, Standard_B1s).

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Aprovisionar el entorno de laboratorio
+ Tarea 2: Configurar la topología de red en estrella tipo hub-and-spoke
+ Tarea 3: Probar la transitividad del emparejamiento de redes virtuales
+ Tarea 4: Configurar el enrutamiento en la topología en estrella tipo hub-and-spoke
+ Tarea 5: Implementar Azure Load Balancer
+ Tarea 6: Implementar Azure Application Gateway

## <a name="estimated-timing-60-minutes"></a>Tiempo estimado: 60 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab06.png)


## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-provision-the-lab-environment"></a>Tarea 1: Aprovisionar el entorno de laboratorio

En esta tarea, implementará cuatro máquinas virtuales en la misma región de Azure. Las dos primeras residirán en una red virtual de centro, mientras que cada una de las dos restantes residirá en una red virtual de radio independiente.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**.

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**.

1. En la barra de herramientas del panel de Cloud Shell, haga clic en el icono **Cargar/Descargar archivos**, haga clic en **Cargar** en el menú desplegable y cargue los archivos **\\Allfiles\\Labs\\06\\az104-06-vms-loop-template.json** y **\\Allfiles\\Labs\\06\\az104-06-vms-loop-parameters.json** en el directorio principal de Cloud Shell.

1. En el panel de Cloud Shell, ejecute lo siguiente para crear el primer grupo de recursos que hospedará el entorno de laboratorio (reemplace el marcador de posición `[Azure_region]` por el nombre de una región de Azure donde tiene pensado implementar las máquinas virtuales de Azure) (puede usar el cmdlet "(Get-AzLocation).Location" para obtener la lista de regiones):

   ```powershell (execute one command at a time)
   $location = '[Azure_region]'

```powershell (execute one command at a time)
   $rgName = 'az104-06-rg1'

```powershell (execute one command at a time)
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. En el panel de Cloud Shell, ejecute lo siguiente para crear las tres redes virtuales y cuatro VM de Azure en ellas mediante los archivos de parámetros y plantilla que cargó:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >**Nota**: Espere a que la implementación se complete antes de continuar con el paso siguiente. Este proceso tardará alrededor de 5 minutos.

1. En el panel de Cloud Shell, ejecute lo siguiente para instalar la extensión Network Watcher en las VM de Azure implementadas en el paso anterior:

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >**Nota**: Espere a que la implementación se complete antes de continuar con el paso siguiente. Este proceso tardará alrededor de 5 minutos.

1. Cierre el panel de Cloud Shell.

#### <a name="task-2-configure-the-hub-and-spoke-network-topology"></a>Tarea 2: Configurar la topología de red en estrella tipo hub-and-spoke

En esta tarea, configurará el emparejamiento local entre las redes virtuales que implementó en las tareas anteriores para crear una topología de red en estrella tipo hub-and-spoke.

1. En Azure Portal, busque y seleccione **Redes virtuales**.

1. Revise las redes virtuales que creó en la tarea anterior.

    >**Nota**: La plantilla que usó para la implementación de las tres redes virtuales asegura que los intervalos de direcciones IP de las tres redes virtuales no se superpongan.

1. En la lista de redes virtuales, seleccione **az104-06-vnet2**.

1. En la hoja **az104-06-vnet2**, seleccione **Propiedades**. 

1. En la hoja **Az104-06-vnet2 \| Propiedades**, registre el valor de la propiedad **Id. de recurso**.

1. Vuelva a la lista de redes virtuales y seleccione **az104-06-vnet3**.

1. En la hoja **az104-06-vnet3**, seleccione **Propiedades**. 

1. En la hoja **Az104-06-vnet3 \| Propiedades**, registre el valor de la propiedad **Id. de recurso**.

    >**Nota**: Necesitará los valores de la propiedad Id. de recurso para ambas redes virtuales más adelante en esta tarea.

    >**Nota**: Se trata de una solución alternativa que aborda el problema por el que Azure Portal en ocasiones no muestra la red virtual recién aprovisionada al crear emparejamientos de redes virtuales.

1. En la lista de redes virtuales, haga clic en **az104-06-vnet01**.

1. En la hoja de la máquina virtual **az104-06-vnet01**, en la sección **Configuración**, haga clic en **Emparejamientos** y luego en **+ Agregar**.

1. Agregue un emparejamiento con las siguientes opciones de configuración (deje las demás con los valores predeterminados) y haga clic en Agregar:

    | Configuración | Value |
    | --- | --- |
    | Esta red virtual: nombre del vínculo de emparejamiento | **az104-06-vnet01_to_az104-06-vnet2** |
    | Tráfico hacia la red virtual remota | **Permitir (predeterminado)** |
    | Tráfico reenviado desde la red virtual remota | **Bloquear el tráfico que se origina fuera de esta red virtual** |
    | Puerta de enlace de red virtual | **Ninguna (valor predeterminado)** |
    | Red virtual remota: nombre del vínculo de emparejamiento | **az104-06-vnet2_to_az104-06-vnet01** |
    | Modelo de implementación de red virtual | **Resource Manager** |
    | Conozco mi Id. de recurso | enabled |
    | Id. de recurso | El valor del parámetro Id. de recurso de **az104-06-vnet2** que registró anteriormente en esta tarea |
    | Tráfico hacia la red virtual remota | **Permitir (predeterminado)** |
    | Tráfico reenviado desde la red virtual remota | **Permitir (predeterminado)** |
    | Puerta de enlace de red virtual | **Ninguna (valor predeterminado)** |

    >**Nota**: Espere a que se complete la operación.

    >**Nota**: Este paso establece dos emparejamientos locales: uno de az104-06-vnet01 a az104-06-vnet2 y el otro de az104-06-vnet2 a az104-06-vnet01.

    >**Nota**: Es necesario habilitar **Permitir tráfico reenviado** para facilitar el enrutamiento entre redes virtuales de radio, que implementará más adelante en este laboratorio.

1. En la hoja de la máquina virtual **az104-06-vnet01**, en la sección **Configuración**, haga clic en **Emparejamientos** y luego en **+ Agregar**.

1. Agregue un emparejamiento con las siguientes opciones de configuración (deje las demás con los valores predeterminados) y haga clic en Agregar:

    | Configuración | Value |
    | --- | --- |
    | Esta red virtual: nombre del vínculo de emparejamiento | **az104-06-vnet01_to_az104-06-vnet3** |
    | Tráfico hacia la red virtual remota | **Permitir (predeterminado)** |
    | Tráfico reenviado desde la red virtual remota | **Bloquear el tráfico que se origina fuera de esta red virtual** |
    | Puerta de enlace de red virtual | **Ninguna (valor predeterminado)** |
    | Red virtual remota: nombre del vínculo de emparejamiento | **az104-06-vnet3_to_az104-06-vnet01** |
    | Modelo de implementación de red virtual | **Resource Manager** |
    | Conozco mi Id. de recurso | enabled |
    | Id. de recurso | El valor del parámetro Id. de recurso de **az104-06-vnet3** que registró anteriormente en esta tarea |
    | Tráfico hacia la red virtual remota | **Permitir (predeterminado)** |
    | Tráfico reenviado desde la red virtual remota | **Permitir (predeterminado)** |
    | Puerta de enlace de red virtual | **Ninguna (valor predeterminado)** |

    >**Nota**: Este paso establece dos emparejamientos locales: uno de az104-06-vnet01 a az104-06-vnet3 y el otro de az104-06-vnet3 a az104-06-vnet01. Esto completa la configuración de la topología en estrella tipo hub-and-spoke (con dos redes virtuales de radio).

    >**Nota**: Es necesario habilitar **Permitir tráfico reenviado** para facilitar el enrutamiento entre redes virtuales de radio, que implementará más adelante en este laboratorio.

#### <a name="task-3-test-transitivity-of-virtual-network-peering"></a>Tarea 3: Probar la transitividad del emparejamiento de redes virtuales

En esta tarea, probará la transitividad del emparejamiento de redes virtuales mediante Network Watcher.

1. En Azure Portal, busque y seleccione **Network Watcher**.

1. En la hoja **Network Watcher**, expanda la lista de regiones de Azure y compruebe que el servicio esté habilitado en la instancia de Azure en la que implementó los recursos en la primera tarea de este laboratorio.

1. Vaya a la hoja **Network Watcher** y luego a **Solución de problemas de conexión**.

1. En la hoja **Network Watcher - Solución de problemas de conexión**, inicie una comprobación con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Tipo de origen | **Máquina virtual** |
    | Máquina virtual | **az104-06-vm0** |
    | Destination | **Especificar manualmente** |
    | URI, FQDN o IPv4 | **10.62.0.4** |
    | Protocolo | **TCP** |
    | Puerto de destino | **3389** |

    > **Note**: **10.62.0.4** representa la dirección IP privada de **az104-06-vm2**.

1. Haga clic en **Comprobar** y espere hasta que se devuelvan los resultados de la comprobación de conectividad. Compruebe que el estado sea **Accesible**. Revise la ruta de acceso de red y observe que la conexión era directa, sin saltos intermedios entre las VM.

    > **Nota**: Esto es lo esperado, ya que la red virtual del centro se empareja directamente con la primera red virtual de radio.

1. En la hoja **Network Watcher - Solución de problemas de conexión**, inicie una comprobación con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Tipo de origen | **Máquina virtual** |
    | Máquina virtual | **az104-06-vm0** |
    | Destination | **Especificar manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocolo | **TCP** |
    | Puerto de destino | **3389** |

    > **Note**: **10.63.0.4** representa la dirección IP privada de **az104-06-vm3**.

1. Haga clic en **Comprobar** y espere hasta que se devuelvan los resultados de la comprobación de conectividad. Compruebe que el estado sea **Accesible**. Revise la ruta de acceso de red y observe que la conexión era directa, sin saltos intermedios entre las VM.

    > **Nota**: Esto es lo esperado, ya que la red virtual del centro se empareja directamente con la segunda red virtual de radio.

1. En la hoja **Network Watcher - Solución de problemas de conexión**, inicie una comprobación con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Tipo de origen | **Máquina virtual** |
    | Máquina virtual | **az104-06-vm2** |
    | Destination | **Especificar manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocolo | **TCP** |
    | Puerto de destino | **3389** |

1. Haga clic en **Comprobar** y espere hasta que se devuelvan los resultados de la comprobación de conectividad. Fíjese que el estado sea **Inaccesible**.

    > **Nota**: Esto es lo esperado, ya que las dos redes virtuales de radio no están emparejadas entre sí (el emparejamiento de redes virtuales no es transitivo).

#### <a name="task-4-configure-routing-in-the-hub-and-spoke-topology"></a>Tarea 4: Configurar el enrutamiento en la topología en estrella tipo hub-and-spoke

En esta tarea, configurará y probará el enrutamiento entre las dos redes virtuales de radio tras habilitar el reenvío de IP en la interfaz de red de la máquina virtual **az104-06-vm0**, habilitar el enrutamiento dentro de su sistema operativo, y configurar rutas definidas por el usuario en la red virtual de radio.

1. En Azure Portal, busque y seleccione **Máquinas virtuales**.

1. En la hoja **Máquinas virtuales**, en la lista de máquinas virtuales, haga clic en **az104-06-vm0**.

1. En la hoja de la máquina virtual **az104-06-vm0**, en la sección **Configuración**, haga clic en **Redes**.

1. Haga clic en el vínculo **az104-06-nic0** junto a la etiqueta **Interfaz de red** y, luego, en la hoja de la interfaz de red **az104-06-nic0**, en la sección **Configuración**, haga clic en **Configuraciones de IP**.

1. Establezca **Reenvío IP** en **Habilitado** y guarde el cambio.

   > **Nota**: Esta configuración es necesaria para que **az104-06-vm0** funcione como enrutador, lo que enruta el tráfico entre dos redes virtuales de radio.

   > **Nota**: Ahora debe configurar el sistema operativo de la máquina virtual **az104-06-vm0** para que admita el enrutamiento.

1. En Azure Portal, vuelva a la hoja de la máquina virtual de Azure **az104-06-vm0** y haga clic en **Información general**.

1. En la hoja **az104-06-vm0**, en la sección **Operaciones**, haga clic en **Ejecutar comando** y, en la lista de comandos, haga clic en **RunPowerShellScript**.

1. En la hoja **Ejecutar script de comando**, escriba lo siguiente y haga clic en **Ejecutar** para instalar el rol de acceso remoto a Windows Server.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Nota**: Espere a que se confirme que el comando se completó correctamente.

1. En la hoja **Ejecutar script de comando**, escriba lo siguiente y haga clic en **Ejecutar** para instalar el servicio de rol Enrutamiento.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Nota**: Espere a que se confirme que el comando se completó correctamente.

   > **Nota**: Ahora debe crear y configurar las rutas definidas por el usuario en las redes virtuales de radio.

1. En Azure Portal, busque y seleccione **Tablas de rutas** y, en la hoja **Tablas de rutas**, haga clic en **+ Crear**.

1. Cree una tabla de rutas con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Location | Nombre de la región de Azure en la que creó las redes virtuales |
    | Name | **az104-06-rt23** |
    | Propagar las rutas de la puerta de enlace | **No** |

1. Haga clic en **Review and Create** (Revisar y crear). Deje que se procese la validación y haga clic en **Crear** para enviar la implementación.

   > **Nota**: Espere a que se cree la tabla de rutas. Este proceso tardará aproximadamente 3 minutos.

1. De nuevo en la hoja **Tablas de rutas**, haga clic en **Actualizar** y, luego en **az104-06-rt23**.

1. En la hoja de la tabla de rutas **az104-06-rt23**, en la sección **Configuración**, haga clic en **Rutas** y luego en **+ Agregar**.

1. Agregue una nueva ruta con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre de ruta | **az104-06-route-vnet2-to-vnet3** |
    | Prefijo de dirección | **10.63.0.0/20** |
    | Tipo de próximo salto | **Aplicación virtual** |
    | Siguiente dirección de salto | **10.60.0.4** |

1. Haga clic en **Aceptar**

1. De nuevo en la hoja de la tabla de rutas **az104-06-rt23**, en la sección **Configuración**, haga clic en **Subredes** y luego en **+ Asociar**.

1. Asocie la tabla de rutas **az104-06-rt23** con la subred siguiente:

    | Configuración | Value |
    | --- | --- |
    | Virtual network | **az104-06-vnet2** |
    | Subnet | **subnet0** |

1. Haga clic en **Aceptar**

1. Vuelva a la hoja **Tablas de rutas** y haga clic en **+ Crear**.

1. Cree una tabla de rutas con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Region | Nombre de la región de Azure en la que creó las redes virtuales |
    | Name | **az104-06-rt32** |
    | Propagar las rutas de la puerta de enlace | **No** |

1. Haga clic en Revisar y crear. Deje que se procese la validación y haga clic en Crear para enviar la implementación.

   > **Nota**: Espere a que se cree la tabla de rutas. Este proceso tardará aproximadamente 3 minutos.

1. De nuevo en la hoja **Tablas de rutas**, haga clic en **Actualizar** y, luego en **az104-06-rt32**.

1. En la hoja de la tabla de rutas **az104-06-rt32**, en la sección **Configuración**, haga clic en **Rutas** y luego en **+ Agregar**.

1. Agregue una nueva ruta con la configuración siguiente:

    | Configuración | Value |
    | --- | --- |
    | Nombre de ruta | **az104-06-route-vnet3-to-vnet2** |
    | Prefijo de dirección | **10.62.0.0/20** |
    | Tipo de próximo salto | **Aplicación virtual** |
    | Siguiente dirección de salto | **10.60.0.4** |

1. Haga clic en **Aceptar**

1. De nuevo en la hoja de la tabla de rutas **az104-06-rt32**, en la sección **Configuración**, haga clic en **Subredes** y luego en **+ Asociar**.

1. Asocie la tabla de rutas **az104-06-rt32** con la subred siguiente:

    | Configuración | Value |
    | --- | --- |
    | Virtual network | **az104-06-vnet3** |
    | Subnet | **subnet0** |

1. Haga clic en **Aceptar**

1. En Azure Portal, vuelva a la hoja **Network Watcher - Solución de problemas de conexión**.

1. En la hoja **Network Watcher - Solución de problemas de conexión**, inicie una comprobación con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | **az104-06-rg1** |
    | Tipo de origen | **Máquina virtual** |
    | Máquina virtual | **az104-06-vm2** |
    | Destination | **Especificar manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocolo | **TCP** |
    | Puerto de destino | **3389** |

1. Haga clic en **Comprobar** y espere hasta que se devuelvan los resultados de la comprobación de conectividad. Compruebe que el estado sea **Accesible**. Revise la ruta de acceso de red y observe que el tráfico se enrutó a través de **10.60.0.4**, asignado al adaptador de red **az104-06-nic0**. Si el estado es **Inaccesible**, debe reiniciar az104-06-vm0.

    > **Nota**: Esto es lo esperado, ya que el tráfico entre redes virtuales de radio ahora se enruta a través de la máquina virtual ubicada en la red virtual del centro, que funciona como enrutador.

    > **Nota**: Puede usar **Network Watcher** para ver la topología de la red.

#### <a name="task-5-implement-azure-load-balancer"></a>Tarea 5: Implementar Azure Load Balancer

En esta tarea, implementará una instancia de Azure Load Balancer delante de las dos máquinas virtuales de Azure en la red virtual del centro.

1. En Azure Portal, busque y seleccione **Equilibradores de carga** y, en la hoja **Equilibradores de carga**, haga clic en **+ Crear**.

1. Cree un equilibrador de carga con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | Nombre de un nuevo grupo de recursos **az104-06-rg1** |
    | Name | **az104-06-lb4** |
    | Region| Nombre de la región de Azure en la que implementó todos los demás recursos de este laboratorio |
    | Tipo | **Public** |
    | SKU | **Estándar** |
    | Dirección IP pública | **Crear nuevo** |
    | Nombre de la dirección IP pública | **az104-06-pip4** |
    | Zona de disponibilidad | **Ninguna zona** |
    | Adición de una dirección IPv6 pública | **No** |

1. Haga clic en Revisar y crear. Deje que se procese la validación y haga clic en Crear para enviar la implementación.

    > **Nota**: Espere a que se aprovisione Azure Load Balancer. Este proceso tardará alrededor de 2 minutos.

1. En la hoja de implementación, haga clic en **Ir al recurso**.

1. En la hoja del equilibrador de carga **az104-06-lb4**, en la sección **Configuración**, haga clic en **Grupos de back-end** y luego en **+ Agregar**.

1. Agregue un grupo de back-end con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre | **az104-06-lb4-be1** |
    | Virtual network | **az104-06-vnet01** |
    | Versión de la dirección IP | **IPv4** |
    | Máquina virtual | **az104-06-vm0** |
    | Dirección IP de la máquina virtual | **ipconfig1 (10.60.0.4)** |
    | Máquina virtual | **az104-06-vm1** |
    | Dirección IP de la máquina virtual | **ipconfig1 (10.60.1.4)** |

1. Haga clic en **Agregar**.

1. Espere a que se cree el grupo de back-end, en la sección **Configuración**, haga clic en **Sondeos de estado** y, a continuación, haga clic en **+ Agregar**.

1. Agregue un sondeo de estado con las opciones de configuración siguientes:

    | Configuración | Value |
    | --- | --- |
    | Nombre | **az104-06-lb4-hp1** |
    | Protocolo | **TCP** |
    | Port | **80** |
    | Intervalo | **5** |
    | Umbral incorrecto | **2** |

1. Haga clic en **Agregar**.

1. Espere a que se cree el sondeo de estado, en la sección **Configuración**, haga clic en **Reglas de equilibrio de carga** y, a continuación, haga clic en **+ Agregar**.

1. Agregue una regla de equilibrio de carga con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre | **az104-06-lb4-lbrule1** |
    | Versión de la dirección IP | **IPv4** |
    | Dirección IP de front-end | **Seleccione LoadBalancerFrontEnd en la lista desplegable**
    | Protocolo | **TCP** |
    | Port | **80** |
    | Puerto back-end | **80** |
    | Grupo back-end | **az104-06-lb4-be1** |
    | Sondeo de mantenimiento | **az104-06-lb4-hp1** |
    | Persistencia de la sesión | **None** |
    | Tiempo de espera de inactividad (minutos) | **4** |
    | Restablecimiento de TCP | **Deshabilitada** |
    | IP flotante (Direct Server Return) | **Deshabilitada** |

1. Haga clic en **Agregar**.

1. Espere a que se cree la regla de equilibrio de carga, en la sección **Configuración**, haga clic en **Configuración de IP de front-end** y anote el valor de la **Dirección IP pública**.

1. Abra otra ventana del explorador y vaya a la dirección IP que identificó en el paso anterior.

1. Compruebe que en la ventana del explorador se muestre el mensaje **Hola mundo de az104-06-vm0** u **Hola mundo de az104-06-vm1**.

1. Abra otra ventana del explorador, pero esta vez desde el modo InPrivate, y compruebe si la VM de destino cambia (como se indica en el mensaje).

    > **Nota**: Es posible que tenga que actualizar la ventana del explorador o abrirla de nuevo desde el modo InPrivate.

#### <a name="task-6-implement-azure-application-gateway"></a>Tarea 6: Implementar Azure Application Gateway

En esta tarea, implementará una instancia de Azure Application Gateway delante de las dos máquinas virtuales de Azure en las redes virtuales de radio.

1. En Azure Portal, busque y seleccione **Redes virtuales**.

1. En la hoja **Redes virtuales**, en la lista de redes virtuales, haga clic en **az104-06-vnet01**.

1. En la hoja de la red virtual **az104-06-vnet01**, en la sección **Configuración**, haga clic en **Subredes** y luego en **+ Subred**.

1. Agregue una subred con las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre | **subnet-appgw** |
    | Intervalo de direcciones de subred | **10.60.3.224/27** |

1. Haga clic en **Guardar**

    > **Nota**: Las instancias de Azure Application Gateway, que implementará más adelante en esta tarea, usarán esta subred. Application Gateway requiere una subred dedicada de tamaño /27 o mayor.

1. En Azure Portal, busque y seleccione **Puertas de enlace de aplicaciones** y, en la hoja **Puertas de enlace de aplicaciones**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear una puerta de enlace de aplicaciones**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Subscription | Nombre de la suscripción de Azure que está usando en este laboratorio |
    | Resource group | Nombre de un nuevo grupo de recursos **az104-06-rg1** |
    | Nombre de la puerta de enlace de aplicaciones | **az104-06-appgw5** |
    | Region | Nombre de la región de Azure en la que implementó todos los demás recursos de este laboratorio |
    | Nivel | **Standard V2** |
    | Habilitación del escalado automático | **No** |
    | HTTP2 | **Deshabilitada** |
    | Virtual network | **az104-06-vnet01** |
    | Subnet | **subnet-appgw** |

1. Haga clic en **Siguiente: Front-ends >** y, en la pestaña **Front-ends** de la hoja **Crear una puerta de enlace de aplicaciones**, haga clic en **Agregar nuevo** y configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Tipo de dirección IP de front-end | **Public** |
    | Dirección IP pública| Nombre de una nueva dirección IP pública **az104-06-pip5** |

1. Haga clic en **Siguiente: Back-ends >** y, en la pestaña **Back-ends** de la hoja **Crear una puerta de enlace de aplicaciones**, haga clic en **Agregar un grupo de back-end** y, en la hoja **Agregar un grupo de back-end**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre | **az104-06-appgw5-be1** |
    | Adición de un grupo de back-end sin destinos | **No** |
    | Tipo de destino | **Dirección IP o FQDN** |
    | Destino | **10.62.0.4** |
    | Tipo de destino | **Dirección IP o FQDN** |
    | Destino | **10.63.0.4** |

    > **Nota**: Los destinos representan las direcciones IP privadas de las máquinas virtuales en las redes virtuales de radio **az104-06-vm2** y **az104-06-vm3**.

1. Haga clic en **Agregar**, haga clic en **Siguiente: Configuración >** y, en la pestaña **Configuración** de la hoja **Crear una puerta de enlace de aplicaciones**, haga clic en **+ Agregar una regla de enrutamiento**.

1. En la hoja **Agregar una regla de enrutamiento**, en la pestaña **Agente de escucha**, escriba los valores siguientes:

    | Configuración | Value |
    | --- | --- |
    | Nombre de la regla | **az104-06-appgw5-rl1** |
    | Nombre del cliente de escucha | **az104-06-appgw5-rl1l1** |
    | Dirección IP de front-end | **Public** |
    | Protocolo | **HTTP** |
    | Port | **80** |
    | Tipo de cliente de escucha | **Basic** |
    | Dirección URL de la página de errores | **No** |

1. Cambie a la pestaña **Destinos de back-end** de la hoja **Agregar una regla de enrutamiento** y configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Tipo de destino | **Grupo de back-end** |
    | Destino de back-end | **az104-06-appgw5-be1** |

1. Haga clic en **Agregar nuevo** bajo el cuadro de texto **Configuración HTTP** y, en la hoja **Agregar una configuración HTTP**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Nombre de la configuración HTTP | **az104-06-appgw5-http1** |
    | Protocolo de back-end | **HTTP** |
    | Puerto back-end | **80** |
    | Afinidad basada en cookies | **Deshabilitar** |
    | Purga de la conexión | **Deshabilitar** |
    | Tiempo de espera de la solicitud (segundos) | **20** |

1. Haga clic en **Agregar** en la hoja **Agregar una configuración HTTP** y, de nuevo, en la hoja **Agregar una regla de enrutamiento**, haga clic en **Agregar**.

1. Haga clic en **Siguiente: Etiquetas >** , seguido de **Siguiente: Revisar y crear** y, luego, haga clic en **Crear**.

    > **Nota**: Espere a que se cree la instancia de Application Gateway. Esto puede tardar unos 8 minutos.

1. En Azure Portal, busque y seleccione **Puertas de enlace de aplicaciones** y, en la hoja **Puertas de enlace de aplicaciones**, haga clic en **az104-06-appgw5**.

1. En la hoja de Application Gateway **az104-06-appgw5**, anote el valor de la **Dirección IP pública de front-end**.

1. Abra otra ventana del explorador y vaya a la dirección IP que identificó en el paso anterior.

1. Compruebe que en la ventana del explorador se muestre el mensaje **Hola mundo de az104-06-vm2** u **Hola mundo de az104-06-vm3**.

1. Abra otra ventana del explorador, pero esta vez desde el modo InPrivate, y compruebe si la VM de destino cambia (según el mensaje que aparece en la página web).

    > **Nota**: Es posible que tenga que actualizar la ventana del explorador o abrirla de nuevo desde el modo InPrivate.

    > **Nota**: Tener como destino máquinas virtuales en varias redes virtuales no es una configuración común, pero tiene como fin ilustrar el grado en el que Application Gateway es capaz de dirigirse a máquinas virtuales en varias redes virtuales (así como a puntos de conexión en otras regiones de Azure o, incluso, fuera de Azure), a diferencia de Azure Load Balancer, que equilibra la carga entre las máquinas virtuales de la misma red virtual.

#### <a name="clean-up-resources"></a>Limpieza de recursos

   >**Nota**: No olvide quitar los recursos de Azure recién creados que ya no use. La eliminación de los recursos sin usar garantiza que no verá cargos inesperados.

1. En Azure Portal, abra la sesión de **PowerShell** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro -AsJob). Aunque podrá ejecutar otro comando de PowerShell inmediatamente después en la misma sesión de PowerShell, los grupos de recursos tardarán unos minutos en eliminarse.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

+ Aprovisionado el entorno de laboratorio
+ Configurado la topología de red en estrella tipo hub-and-spoke
+ Probado la transitividad del emparejamiento de redes virtuales
+ Configurado el enrutamiento en la topología en estrella tipo hub-and-spoke
+ Implementado Azure Load Balancer
+ Implementado Azure Application Gateway
