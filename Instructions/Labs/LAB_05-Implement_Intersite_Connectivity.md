---
lab:
  title: 'Laboratorio 05: Implementación de la conectividad entre sitios'
  module: Administer Intersite Connectivity
---

# Laboratorio 05: Implementación de la conectividad entre sitios

## Introducción al laboratorio

En este laboratorio, explorará la comunicación entre redes virtuales. Implemente el emparejamiento de redes virtuales y pruebe las conexiones. También creará una ruta personalizada. 

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para **Este de EE. UU.** 

## Tiempo estimado: 50 minutes
    
## Escenario del laboratorio 

Su organización segmenta los servicios y aplicaciones de TI principales (como DNS y servicios de seguridad) de otras partes de la empresa, incluido el departamento de fabricación. Sin embargo, en algunos escenarios, las aplicaciones y los servicios del área principal necesitan comunicarse con aplicaciones y servicios en el área de fabricación. En este laboratorio, configurará la conectividad entre las áreas segmentadas. Este es un escenario común para separar la producción del desarrollo o separar una filial de otra.  

## Simulaciones interactivas de laboratorio

>**Nota**: las simulaciones de laboratorio proporcionadas anteriormente se han retirado.

## Diagrama de arquitectura

![Diagrama de arquitectura del laboratorio 05](../media/az104-lab05-architecture.png)

## Aptitudes de trabajo

+ Tarea 1: Cree una máquina virtual en una red virtual.
+ Tarea 2: Cree una máquina virtual en otra red virtual.
+ Tarea 3: Utilice Network Watcher para probar la conexión entre máquinas virtuales. 
+ Tarea 4: Configure emparejamientos de red virtual entre diferentes redes virtuales.
+ Tarea 5: Use Azure PowerShell para probar la conexión entre máquinas virtuales.
+ Tarea 6: Crear una ruta personalizada. 

## Tarea 1:  Creación de una máquina virtual de servicios principales y una red virtual

En esta tarea, creará una red virtual de servicios principales con una máquina virtual. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `Virtual Machines`.

1. En la página máquinas virtuales, seleccione **Crear**, después, seleccione **Azure Virtual Machine**.

1. En la pestaña Aspectos básicos, use la siguiente información para completar el formulario y, a continuación, seleccione **Siguiente: Discos >**. Para cualquier configuración no especificada, deje el valor predeterminado.
 
    | Configuración | Valor | 
    | --- | --- |
    | Suscripción |  *tu suscripción* |
    | Resource group |  `az104-rg5` (Si es necesario, **Crear nuevo**. )
    | Nombre de la máquina virtual |    `CoreServicesVM` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Opciones de disponibilidad | No se requiere redundancia de la infraestructura |
    | Tipo de seguridad | **Estándar** |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** (observe las otras opciones) |
    | Size | **Standard_DS2_v3** |
    | Nombre de usuario | `localadmin` | 
    | Contraseña | **Proporcionar una contraseña compleja** |
    | Puertos de entrada públicos | **Ninguno** |

    ![Captura de pantalla de la página de creación de máquinas virtuales básicas. ](../media/az104-lab05-createcorevm.png)
   
1. En la pestaña **Disk**, tome los valores predeterminados y, a continuación, seleccione **Siguiente: Redes >**.

1. En la pestaña **Redes**, en Red virtual, seleccione **Crear nuevo**.

1. Usa la siguiente información para configurar la red virtual y, a continuación, selecciona **Aceptar**. Si es necesario, quite o reemplace la información existente.

    | Configuración | Valor | 
    | --- | --- |
    | Nombre | `CoreServicesVnet` (Crear nuevo) |
    | Intervalo de direcciones | `10.0.0.0/16`  |
    | Nombre de subred | `Core` | 
    | Intervalo de direcciones de subred | `10.0.0.0/24` |

1. Seleccione la pestaña **Supervisión**. En Diagnósticos de arranque, seleccione **Deshabilitar**.

1. Seleccione **Revisar y crear** y, luego, **Crear**.

1. No es necesario esperar a que se creen los recursos. Continúe con la siguiente tarea.

    >**Nota:** ¿Ha observado que en esta tarea creó la red virtual a medida que creó la máquina virtual? También puede crear la infraestructura de red virtual y, a continuación, agregar las máquinas virtuales. 

## Tarea 2: Cree una máquina virtual en otra red virtual

En esta tarea, creará una red virtual de servicios principales con una máquina virtual. 

1. En Azure Portal, busque y seleccione **Máquinas virtuales**.

1. En la página máquinas virtuales, seleccione **Crear**, después, seleccione **Azure Virtual Machine**.

1. En la pestaña Aspectos básicos, use la siguiente información para completar el formulario y, a continuación, seleccione **Siguiente: Discos >**. Para cualquier configuración no especificada, deje el valor predeterminado.
 
    | Configuración | Valor | 
    | --- | --- |
    | Suscripción |  *tu suscripción* |
    | Resource group |  `az104-rg5` |
    | Nombre de la máquina virtual |    `ManufacturingVM` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Tipo de seguridad | **Estándar** |
    | Opciones de disponibilidad | No se requiere redundancia de la infraestructura |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Size | **Standard_DS2_v3** | 
    | Nombre de usuario | `localadmin` | 
    | Contraseña | **Proporcionar una contraseña compleja** |
    | Puertos de entrada públicos | **Ninguno** |

1. En la pestaña **Disk**, tome los valores predeterminados y, a continuación, seleccione **Siguiente: Redes >**.

1. En la pestaña Redes, en Red virtual, seleccione **Crear nuevo**.

1. Usa la siguiente información para configurar la red virtual y, a continuación, selecciona **Aceptar**.  Si es necesario, quite o reemplace el intervalo de direcciones existente.

    | Configuración | Valor | 
    | --- | --- |
    | Nombre | `ManufacturingVnet` |
    | Intervalo de direcciones | `172.16.0.0/16`  |
    | Nombre de subred | `Manufacturing` |
    | Intervalo de direcciones de subred | `172.16.0.0/24` |

1. Seleccione la pestaña **Supervisión**. En Diagnósticos de arranque, seleccione **Deshabilitar**.

1. Seleccione **Revisar y crear** y, luego, **Crear**.

## Tarea 3: Utilice Network Watcher para probar la conexión entre máquinas virtuales 


En esta tarea, comprobará que los recursos de las redes virtuales emparejadas pueden comunicarse entre sí. Network Watcher se usará para probar la conexión. Antes de continuar, asegúrese de que ambas máquinas virtuales se han implementado y se están ejecutando. 

1. En Azure Portal, busque y seleccione `Network Watcher`.

1. En Network Watcher, en el menú Herramientas de diagnóstico de red, seleccione **Solución de problemas de conexión**.

1. Utilice la siguiente información para completar los campos de la página **Solución de problemas de conexión**.

    | Campo | Valor | 
    | --- | --- |
    | Tipo de origen           | **Máquina virtual**   |
    | Máquina virtual       | **CoreServicesVM**    | 
    | Tipo de destino      | **Máquina virtual**   |
    | Máquina virtual       | **ManufacturingVM**   | 
    | Versión de IP preferida  | **Ambos**              | 
    | Protocolo              | **TCP**               |
    | Puerto de destino      | `3389`                |  
    | Puerto de origen           | *Blank*         |
    | Pruebas de diagnóstico      | *Defaults*      |

    ![Azure Portal muestra la configuración de solución de problemas de conexión.](../media/az104-lab05-connection-troubleshoot.png)

1. Seleccione **Ejecutar pruebas de diagnóstico**.

    >**Nota**: Los resultados pueden tardar un par de minutos en devolverse. Las selecciones de pantalla se atenuarán mientras se recopilan los resultados. Observe que la **Prueba de conectividad** muestra **UnReachable**. Esto tiene sentido porque las máquinas virtuales están en diferentes redes virtuales. 

 
## Tarea 4: Configuración de emparejamientos de redes virtuales entre redes virtuales

En esta tarea, creará un emparejamiento de red virtual para habilitar las comunicaciones entre los recursos de las redes virtuales. 

1. En Azure Portal, seleccione la `CoreServicesVnet` red virtual.

1. En CoreServicesVnet, en **Configuración**, seleccione **Emparejamientos**.

1. En CoreServicesVnet, en Emparejamientos, selecciona **+ Agregar**. Si no se especifica, tome el valor predeterminado. 

    | **Parámetro**                                    | **Valor**                             |
    | --------------------------------------------- | ------------------------------------- |                                
    | Nombre del vínculo de emparejamiento                             | `CoreServicesVnet-to-ManufacturingVnet` |
    | Red virtual    | **ManufacturingVM-net (az104-rg5)**  |
    | Permitir que ManufacturingVNet acceda a CoreServicesVNet  | seleccionado (valor predeterminado) |
    | Permitir que ManufacturingVnet reciba tráfico reenviado desde CoreServicesVnet | seleccionados  |
    | Nombre del vínculo de emparejamiento                             | `ManufacturingVnet-to-CoreServicesVnet` |
    | Permitir que CoreServicesVNet acceda a la red virtual emparejada            | seleccionado (valor predeterminado) |
    | Permitir que CoreServicesVNet reciba tráfico reenviado de la red virtual emparejada | seleccionados |

4. Haz clic en **Agregar**.

5. En CoreServicesVnet, en Emparejamientos, comprueba que se muestra el emparejamiento **CoreServicesVnet-to-ManufacturingVnet**. Actualice la página para asegurarse de que el **estado de emparejamiento** es **Conectado**.

6. Cambie al **ManufacturingVnet** y compruebe que ** se muestra el emparejamiento ManufacturingVnet-to-CoreServicesVnet**. Asegúrese de que el **Estado de emparejamiento** es **Conectado**. Es posible que tenga que **actualizar** la página. 

## Tarea 5: Uso de Azure PowerShell para probar la conexión entre máquinas virtuales

En esta tarea, se vuelve a probar la conexión entre las máquinas virtuales de diferentes redes virtuales. 

### Comprobación de la dirección IP privada de CoreServicesVM

1. En Azure Portal, busque y seleccione la `CoreServicesVM` máquina virtual.

1. En la hoja **Información general**, en la sección **Redes**, registre la **dirección IP privada** del equipo. Necesita esta información para probar la conexión.
   
### Pruebe la conexión a CoreServicesVM desde el **ManufacturingVM**.

>**¿Sabía que...?** Hay muchas maneras de comprobar las conexiones. En esta tarea, usará comando **Ejecutar**. También puede seguir usando Network Watcher. O bien, podría usar un [Conexión a Escritorio remoto](https://learn.microsoft.com/azure/virtual-machines/windows/connect-rdp#connect-to-the-virtual-machine) para acceder a la máquina virtual. Una vez conectado, use **test-connection**. Cuando tenga tiempo, pruebe RDP. 

1. Cambie a la `ManufacturingVM` máquina virtual.

1. En la hoja **Operations**, seleccione la hoja **Ejecutar comando**.

1. Seleccione **runPowerShellScript** y ejecute el comando **Test-NetConnection**. Asegúrese de usar la dirección IP privada del **CoreServicesVM**.

    ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
    ```
1. El script de comandos puede tardar un par de minutos en finalizar. En la parte superior de la página se muestra un mensaje informativo *Ejecución de script en curso*.

   
1. La conexión de prueba debe realizarse correctamente porque se ha configurado el emparejamiento. El nombre del equipo y la dirección remota de este gráfico pueden ser diferentes. 
   
   ![Ventana de PowerShell con Test-NetConnection correcta.](../media/az104-lab05-success.png)

## Tarea 6: Creación de una ruta personalizada 

En esta tarea, desea controlar el tráfico de red entre la subred perimetral y la subred de servicios centrales internos. Se instalará una aplicación de red virtual en la subred de servicios principales y todo el tráfico debe enrutarse allí. 

1. Busque seleccionar `CoreServicesVnet`.

1. Selecciona **Subredes** y, a continuación, **+ Subred**. Asegúrate de seleccionar **Agregar** para guardar los cambios. 

    | Configuración | Valor | 
    | --- | --- |
    | Nombre | `perimeter` |
    | Dirección inicial | `10.0.1.0/24`  |

   
1. En Azure Portal, busca y selecciona `Route tables` y, a continuación, selecciona **+ Crear**.

1. Escribe los detalles siguientes, selecciona **Revisar y crear** y, a continuación, selecciona **Crear**. 

    | Configuración | Valor | 
    | --- | --- |
    | Suscripción | su suscripción |
    | Resource group | `az104-rg5`  |
    | Region | **Este de EE. UU.** |
    | Nombre | `rt-CoreServices` |
    | Propagar las rutas de la puerta de enlace | **No** |

1. Después de implementar la tabla de rutas, busca y selecciona las **Tablas de rutas**.
   
1. Selecciona el recurso (no la casilla) **rt-CoreServices**

1. Expande **Configuración**, a continuación, selecciona **Rutas** y, después, **Agregar**. Crea una ruta desde la aplicación virtual de red (NVA) futura a la red virtual CoreServices. 

    | Configuración | Value | 
    | --- | --- |
    | Nombre de ruta | `PerimetertoCore` |
    | Tipo de destino | **Direcciones IP** |
    | Direcciones IP de destino | `10.0.0.0/16` (red virtual de servicios principales) |
    | Tipo de próximo salto | **Aplicación virtual** (observe las demás opciones) |
    | Siguiente dirección de salto | `10.0.1.7` (NVA futura) |

1. Seleccione **+Agregar**. Lo último que debe hacer es asociar la ruta a la subred.

1. Selecciona **Subredes** y, a continuación, **+ Asociar**. Complete la configuración.

    | Configuración | Valor | 
    | --- | --- |
    | Virtual network | **CoreServicesVnet** |
    | Subnet | **Principal** |    

>**Nota**: Ha creado una ruta definida por el usuario para dirigir el tráfico desde la red perimetral a la nueva aplicación virtual de red.  

## Limpieza de los recursos

Si utilizas **tu propia suscripción**, dedica un minuto a eliminar los recursos del laboratorio. De esta forma estarás seguro de que los recursos se liberan y de que se minimiza el coste. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, selecciona el grupo de recursos, selecciona **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haz clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarte a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesitas más información. Abre un explorador Edge y elige Copilot (superior derecha) o ve a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ ¿Cómo puedo usar los comandos de Azure PowerShell o la CLI de Azure para agregar un emparejamiento de red virtual entre vnet1 y vnet2?
+ Cree una tabla que resalte varias herramientas de supervisión de Azure y de terceros compatibles con Azure. Resalte cuándo usar cada herramienta. 
+ ¿Cuándo crearía una ruta de red personalizada en Azure?

## Más información con el aprendizaje autodirigido

+ [Distribución de los servicios a través de redes virtuales de Azure e integración de estos mediante emparejamiento de red virtual](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/). Use el emparejamiento de redes virtuales para habilitar la comunicación entre redes virtuales de forma segura y con una complejidad mínima.
+ [Administrar y controlar el flujo de tráfico en la implementación de Azure con rutas](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Aprenda a controlar el tráfico de red virtual de Azure implementando rutas personalizadas.


## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ De forma predeterminada, los recursos de diferentes redes virtuales no se pueden comunicar.
+ El emparejamiento de red virtual permite conectar sin problemas dos o más redes virtuales en Azure.
+ Las redes virtuales compartidas aparecen como una sola a efectos de conectividad.
+ El tráfico entre las máquinas virtuales de la red virtual emparejada usa la infraestructura de la red troncal de Microsoft.
+ Las rutas definidas por el sistema se crean automáticamente para cada subred de una red virtual. Las rutas definidas por el usuario invalidan o agregan a las rutas del sistema predeterminadas. 
+ Azure Network Watcher proporciona un conjunto de herramientas para supervisar, diagnosticar y ver métricas y registros de los recursos de IaaS de Azure.
