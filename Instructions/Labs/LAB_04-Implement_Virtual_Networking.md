---
lab:
  title: "Laboratorio\_04: Implementación de redes virtuales"
  module: Implement Virtual Networking
---

# Laboratorio 04: Implementación de redes virtuales

## Introducción al laboratorio

Este es el primero de tres laboratorios que se centra en las redes virtuales. En este laboratorio, aprenderá los conceptos básicos de las redes virtuales y las subredes. Aprenderá a proteger la red con grupos de seguridad de red y grupos de seguridad de aplicaciones. También obtendrá información sobre las zonas y registros DNS. 

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para **Este de EE. UU.**

## Tiempo estimado: 50 minutes

## Escenario del laboratorio 

Su organización global planea implementar redes virtuales. El objetivo inmediato es dar cabida a todos los recursos existentes. Sin embargo, la organización está en una fase de crecimiento y quiere asegurarse de que hay capacidad adicional para ello.

La red virtual **CoreServicesVnet** tiene el mayor número de recursos. Se prevé una gran cantidad de crecimiento, por lo que se necesita un gran espacio de direcciones para esta red virtual.

La red virtual **ManufacturingVnet** contiene sistemas para las operaciones de las instalaciones de fabricación. La organización prevé un gran número de dispositivos conectados internos para que sus sistemas recuperen datos. 

## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que le podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure. 

+ [Tráfico de red seguro](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013). Cree una máquina virtual, una red virtual y un grupo de seguridad de red. Agregue reglas de grupo de seguridad de red para permitir y no permitir el tráfico.
  
+ [Creación de una red virtual sencilla](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204). Cree una red virtual con dos máquinas virtuales. Demuestre que las máquinas virtuales pueden comunicarse. 

+ [Diseño e implementación de una red virtual en Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Cree un grupo de recursos y cree redes virtuales con subredes.  

+ [Implementación de redes virtuales](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208). Cree y configure una red virtual, implemente máquinas virtuales, configure grupos de seguridad de red y configure Azure DNS.

## Diagrama de la arquitectura

![Diseño de red](../media/az104-lab04-architecture.png)

Estas redes virtuales y subredes están estructuradas de manera que se adaptan a los recursos existentes, pero permiten el crecimiento previsto. A continuación se crearán estas redes virtuales y subredes para sentar las bases de la infraestructura de red.

>**¿Sabía que...?**: Se recomienda evitar la superposición de intervalos de direcciones IP para reducir los problemas y simplificar la solución de problemas. La superposición es un problema en toda la red, ya sea en la nube o en el entorno local. Muchas organizaciones diseñan un esquema de direccionamiento IP para toda la empresa para evitar la superposición y planear el crecimiento futuro.

## Aptitudes de trabajo

+ Tarea 1: Cree una red virtual con subredes mediante el portal.
+ Tarea 2: Cree una red virtual y subredes mediante una plantilla.
+ Tarea 3: Cree y configure la comunicación entre un grupo de seguridad de aplicaciones y un grupo de seguridad de red.
+ Tarea 4: Configure zonas DNS de Azure públicas y privadas.
  
## Tarea 1: Creación de una red virtual con subredes mediante el portal

La organización planea una gran cantidad de crecimiento para los servicios principales. En esta tarea, creará la red virtual y las subredes asociadas para acomodar los recursos existentes y el crecimiento planeado. En esta tarea, usará Azure Portal. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.
   
1. Busque y seleccione `Virtual Networks`.

1. En la página Redes virtuales, seleccione **Crear**.

1. Complete la pestaña **Aspectos básicos** de CoreServicesVnet.  

    |  **Opción**         | **Valor**            |
    | ------------------ | -------------------- |
    | Grupo de recursos     | `az104-rg4` (si es necesario, cree uno nuevo) |
    | Nombre               | `CoreServicesVnet`     |
    | Región             | (EE. UU.) **Este de EE. UU.**         |

1. Vaya a la pestaña **Direcciones IP**.

    |  **Opción**         | **Valor**            |
    | ------------------ | -------------------- |
    | Espacio de direcciones IPv4 | Reemplace el espacio de direcciones IPv4 rellenado previamente por `10.20.0.0/16` (separe las entradas)  |

1. Seleccione **+ Agregar una subred**. Complete la información de nombre y dirección de cada subred. Asegúrese de seleccionar **Agregar** para cada nueva subred. Asegúrese de eliminar la subred predeterminada, ya sea antes o después de crear las otras subredes.

    | **Subred**             | **Opción**           | **Valor**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nombre de subred          | `SharedServicesSubnet`   |
    |                        | Dirección inicial     | `10.20.10.0`          |
    |                        | Size                 | `/24` |
    | DatabaseSubnet         | Nombre de subred          | `DatabaseSubnet`         |
    |                        | Dirección inicial     | `10.20.20.0`        |
    |                        | Size                 | `/24` |

    >**Nota:** Cada red virtual debe tener al menos una subred. Recuerde que siempre se reservarán cinco direcciones IP, así que téngalo en cuenta en la planificación. 

1. Para terminar de crear la red virtual CoreServicesVnet y las subredes asociadas, seleccione **Revisar y crear**.

1. Compruebe que la configuración superó la validación y, luego, seleccione **Crear**.

1. Espere a que se implemente la red virtual y seleccione **Ir al recurso**.

1. Dedique un minuto a comprobar el **Espacio de direcciones** y las **Subredes**. Observe las demás opciones en la hoja **Configuración**. 

1. En la sección **Automatización**, seleccione **Exportar plantilla** y espere a que se genere la plantilla.

1. **Descargue** la plantilla.

1. Vaya a la máquina local a la carpeta **Descargas** y **Extraiga todos** los archivos del archivo ZIP descargado. 

1. Antes de continuar, asegúrese de que tiene el archivo **template.json**. Usará esta plantilla para crear la ManufacturingVnet en la siguiente tarea. 
 
## Tarea 2: Creación de una red virtual y subredes mediante una plantilla

En esta tarea, creará la red virtual ManufacturingVnet y las subredes asociadas. La organización prevé el crecimiento de las oficinas de fabricación, por lo que las subredes están dimensionadas para el crecimiento previsto. Para esta tarea, se usa una plantilla para crear los recursos. 

1. Busque el archivo **template.json** exportado en la tarea anterior. Debe estar en la carpeta **Descargas**.

1. Edite el archivo con el editor que prefiera. Muchos editores tienen una característica de *cambio de todas las apariciones*. Si usa Visual Studio Code, asegúrese de que está trabajando en una **ventana de confianza** y no en el **modo restringido**. Consulte el diagrama de arquitectura para comprobar los detalles. 

### Realización de cambios en la red virtual ManufacturingVnet

1. Reemplace todas las apariciones de **CoreServicesVnet** por `ManufacturingVnet`. 

1. Reemplace todas las apariciones de **10.20.0.0** por `10.30.0.0`. 

### Realización de cambios en las subredes de ManufacturingVnet

1. Cambie todas las apariciones de **SharedServicesSubnet** a `SensorSubnet1`.

1. Cambie todas las apariciones de **10.20.10.0/24** a `10.30.20.0/24`.

1. Cambie todas las apariciones de **DatabaseSubnet** a `SensorSubnet2`.

1. Cambie todas las apariciones de **10.20.20.0/24** a `10.30.21.0/24`.

1. Vuelva a leer el archivo y asegúrese de que todo es correcto.

1. Asegúrese de **Guardar** los cambios.

>**Nota:** Hay archivos de plantilla completados en el directorio de archivos de laboratorio. 

### Realización de cambios en el archivo de parámetros

1. Localice el archivo **parameters.json** exportado en la tarea anterior. Debe estar en la carpeta **Descargas**.

1. Edite el archivo con el editor que prefiera.

1. Reemplace una aparición de **CoreServicesVnet** por `ManufacturingVnet`.

1. Guarde los cambios mediante **Guardar**.
   
### Implementación de la plantilla personalizada

1. En el portal, busque y seleccione **Implementar una plantilla personalizada**.

1. Seleccione **Compilar su propia plantilla en el editor** y, a continuación, **Cargar archivo**.

1. Seleccione el archivo **templates.json** con los cambios de fabricación y, a continuación, seleccione **Guardar**.

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.

1. Espere a que se implemente la plantilla y, a continuación, confirme (en el portal) que se han creado la red virtual de fabricación y las subredes.

>**Nota:** Si tiene que implementar más de una vez, puede encontrar que algunos recursos se completaron correctamente y se produce un error en la implementación. Puede quitar manualmente esos recursos e intentarlo de nuevo. 
   
## Tarea 3: Cree y configure la comunicación entre un grupo de seguridad de aplicaciones y un grupo de seguridad de red

En esta tarea, creamos un grupo de seguridad de aplicaciones y un grupo de seguridad de red. El grupo de seguridad de red tendrá una regla de seguridad de entrada que permita el tráfico desde el ASG. El grupo de seguridad de red también tendrá una regla de salida que deniega el acceso a Internet. 

### Creación del grupo de seguridad de aplicaciones (ASG)

1. En Azure Portal, busque y seleccione `Application security groups`.

1. Haga clic en **Crear** y proporcione la información básica.

    | Configuración | Valor |
    | -- | -- |
    | Suscripción | *su suscripción* |
    | Resource group | **az104-rg4** |
    | Nombre | `asg-web` |
    | Region | **Este de EE. UU.**  |

1. Haga clic en **Revisar y crear** y luego, después de la validación, haga clic en **Crear**.

### Cree el grupo de seguridad de red y asócielo a la subred del ASG

1. En Azure Portal, busque y seleccione `Network security groups`.

1. Seleccione **+ Crear** y proporcione información sobre la pestaña **Conceptos básicos**. 

    | Configuración | Valor |
    | -- | -- |
    | Suscripción | *su suscripción* |
    | Resource group | **az104-rg4** |
    | Nombre | `myNSGSecure` |
    | Region | **Este de EE. UU.**  |

1. Haga clic en **Revisar y crear** y luego, después de la validación, haga clic en **Crear**.

1. Una vez implementado el grupo de seguridad de red, haga clic en **Ir al recurso**.

1. En **Configuración** haga clic en **Subredes** y, a continuación, **Asociar**.

    | Configuración | Value |
    | -- | -- |
    | Virtual network | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |

1. Haga clic en **Aceptar** para guardar la asociación.

### Configuración de una regla de seguridad de entrada para permitir el tráfico del ASG

1. Continúe trabajando con el grupo de seguridad de red. En el área **Configuración**, seleccione **Reglas de seguridad de entrada**.

1. Revise las reglas de entrada predeterminadas. Observe que solo se permite el acceso a otras redes virtuales y equilibradores de carga.

1. Seleccione **+Agregar**.

1. En la hoja **Agregar regla de seguridad de entrada**, use la siguiente información para agregar una regla de puerto de entrada. Esta regla permite el tráfico del ASG. Cuando haya terminado, seleccione **Agregar**.

    | Configuración | Valor |
    | -- | -- |
    | Source | **Grupo de seguridad de aplicaciones** |
    | Grupos de seguridad de la aplicación de origen | **asg-web** |
    | Rangos del puerto origen |  * |
    | Destino | **Cualquiera** |
    | Service | **Personalizada** (observe las otras opciones) |
    | Intervalos de puertos de destino | **80, 443** |
    | Protocolo | **TCP** |
    | Acción | **Permitir** |
    | Prioridad | **100** |
    | Nombre | `AllowASG` |

### Configuración de una regla del NSG saliente que deniega el acceso a Internet

1. Después de crear la regla del NSG de entrada, seleccione **Reglas de seguridad de salida**. 

1. Observe la regla **AllowInternetOutboundRule**. Observe también que la regla no se puede eliminar y la prioridad es 65001.

1. Seleccione **+ Agregar** y configure una regla de salida que deniegue el acceso a Internet. Cuando haya terminado, seleccione **Agregar**.

    | Configuración | Valor |
    | -- | -- |
    | Origen | **Cualquiera** |
    | Rangos del puerto origen |  * |
    | Destination | **Etiqueta de servicio** |
    | Etiqueta de servicio de destino | **Internet** |
    | Service | **Personalizada** |
    | Intervalos de puertos de destino | **8080** |
    | Protocolo | **Cualquiera** |
    | Acción | **Deny** |
    | Prioridad | **4096** |
    | Nombre | **DenyAnyCustom8080Outbound** |


## Tarea 4: Configuración de zonas DNS de Azure públicas y privadas

En esta tarea, creará y configurará zonas DNS públicas y privadas. 

### Configuración de una zona DNS pública

Puede configurar Azure DNS para resolver nombres de host en el dominio público. Por ejemplo, si ha adquirido el nombre de dominio contoso.xyz de un registrador de nombres de dominio, puede configurar Azure DNS para hospedar el dominio `contoso.com` y resolver www.contoso.xyz en la dirección IP del servidor web o la aplicación web.

1. En el portal, busque y seleccione `DNS zones`.

1. Seleccione **+ Create** (+ Crear).

1. Configure la pestaña **Aspectos básicos**.

    | Propiedad | Valor    |
    |:---------|:---------|
    | Suscripción | **Seleccione la suscripción** |
    | Resource group | **az-104-rg4** |
    | Nombre | `contoso.com` (si se reserva, ajuste el nombre) |
    | Region |**Este de EE. UU.** (revise el icono informativo) |

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.
   
1. Espere a que se implemente la zona DNS y seleccione **Ir al recurso**.

1. En la hoja **Información general**, observe los nombres de los cuatro servidores de nombres DNS de Azure asignados a la zona. **Copie** una de las direcciones del servidor de nombres. La necesitará en un paso posterior. 
  
1. Seleccione **+ Conjunto de registros**. Agregue un registro de vínculo de red virtual para cada red virtual que necesite compatibilidad con la resolución de nombres privados.

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre | **www** |
    | Tipo | **A**
           |
    | TTL | **1** |
    | Dirección IP | **10.1.1.4** |

>**Nota:**  En un escenario real, escribiría la dirección IP pública del servidor web.

1. Seleccione **Aceptar** y compruebe que **contoso.com** tiene un conjunto de registros A denominado **www**.

1. Abra un símbolo del sistema y ejecute el comando siguiente:

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Compruebe que el nombre de host www.contoso.com se resuelve en la dirección IP proporcionada. Esto confirma que la resolución de nombres funciona correctamente.

### Configuración de una zona DNS privada

Una zona DNS privada proporciona servicios de resolución de nombres dentro de las redes virtuales. Solo se puede acceder a una zona DNS privada desde las redes virtuales a las que está vinculada y no se puede acceder desde Internet. 

1. En el portal, busque y seleccione `Private dns zones`.

1. Seleccione **+ Create** (+ Crear).

1. En la pestaña **Datos básicos** de Crear zona DNS privada, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    | Suscripción | **Seleccione la suscripción** |
    | Resource group | **az-104-rg4** |
    | Nombre | `private.contoso.com` (ajuste si tenía que cambiar el nombre) |
    | Region |**Este de EE. UU.** |

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.
   
1. Espere a que se implemente la zona DNS y seleccione **Ir al recurso**.

1. Observe que en la hoja **Información general** no hay registros de servidor de nombres. 

1. Seleccione **+ Vínculos de red virtual** y, después, seleccione **+ Agregar**. 

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre del vínculo | `manufacturing-link` |
    | Virtual network | `ManufacturingVnet` |

1. Seleccione **Aceptar** y espere a que se cree el vínculo. 

1. En la hoja **Información general**, seleccione **+ Conjunto de registros**. Ahora agregaría un registro para cada máquina virtual que necesite compatibilidad con la resolución de nombres privada.

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre | **sensorvm** |
    | Tipo | **A**
           |
    | TTL | **1** |
    | Dirección IP | **10.1.1.4** |

 >**Nota:**  En un escenario real, escribiría la dirección IP de una máquina virtual de fabricación específica.

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot

Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.
+ Comparta los 10 mejores procedimientos recomendados al implementar y configurar una red virtual en Azure.
+ Cómo uso los comandos de Azure PowerShell y la CLI de Azure para crear una red virtual con una dirección IP pública y una subred. 
+ Explicar las reglas de entrada y salida del grupo de seguridad de red de Azure y cómo se usan.
+ ¿Cuál es la diferencia entre los grupos de seguridad de red de Azure y los grupos de seguridad de aplicaciones de Azure? Comparta ejemplos de cuándo usar cada uno de estos grupos. 
+ Proporcione una guía paso a paso sobre cómo solucionar los problemas de red que se enfrentan al implementar una red en Azure. Comparta también el proceso de pensamiento usado para cada paso de la solución de problemas.

## Más información con el aprendizaje autodirigido

+ [Introducción a las redes virtuales de Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Diseñe e implemente la infraestructura principal de Redes de Azure, como redes virtuales, direcciones IP públicas y privadas, DNS, emparejamiento de red virtual, enrutamiento y Azure Virtual NAT.
+ [Diseñe un esquema de direccionamiento IP](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/). Identifique las funcionalidades de direcciones IP públicas y privadas de Azure y las redes virtuales locales.
+ [Proteja y aísle el acceso a recursos de Azure mediante grupos de seguridad de red y puntos de conexión de servicio](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). Los grupos de seguridad de red y los puntos de conexión de servicio ayudan a proteger las máquinas virtuales y los servicios de Azure contra al acceso de red no autorizado.
+ [Hospede el dominio en Azure DNS](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Cree una zona DNS para su nombre de dominio. Cree registros DNS para asignar el dominio a una dirección IP. Compruebe que el nombre de dominio se resuelve en su servidor web.
  
## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Una red virtual es una representación de su propia red en la nube. 
+ Al diseñar redes virtuales, es recomendable evitar que se superponga intervalos de direcciones IP. Esto reducirá los problemas y simplificará la solución de problemas.
+ Una subred es un intervalo de direcciones IP en la red virtual. Una red virtual se puede dividir en varias subredes para facilitar su organización o por motivos de seguridad.
+ Un grupo de seguridad de red contiene reglas de seguridad que permiten o deniegan el tráfico de red. Hay reglas entrantes y salientes predeterminadas que puede personalizar para sus necesidades.
+ Los grupos de seguridad de aplicaciones se usan para proteger grupos de servidores con una función común, como servidores web o servidores de bases de datos.
+ Azure DNS es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres. Puede configurar Azure DNS para resolver nombres de host en el dominio público.  También puede usar zonas DNS privadas para asignar nombres DNS a máquinas virtuales (VM) en las redes virtuales de Azure.
