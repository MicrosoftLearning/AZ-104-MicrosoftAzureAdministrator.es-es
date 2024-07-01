---
lab:
  title: 'Laboratorio 01: administrar identidades de Microsoft Entra ID'
  module: Administer Identity
---

# Laboratorio 01: administrar identidades de Microsoft Entra ID

## Introducción al laboratorio

Este es el primero de una serie de laboratorios para administradores de Azure. En este laboratorio, obtendrá información sobre usuarios y grupos. Los usuarios y grupos son los bloques de creación básicos de una solución de identidad. 

Este laboratorio requiere una suscripción a Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para **Este de EE. UU.** 


## Tiempo estimado: 30 minutos

## Escenario del laboratorio

Su organización está creando un nuevo entorno de laboratorio para pruebas de preproducción de aplicaciones y servicios.  Algunos ingenieros se están contratando para administrar el entorno de laboratorio, incluidas las máquinas virtuales. Para permitir que los ingenieros se autentiquen mediante el Microsoft Entra ID, se le ha encargado el aprovisionamiento de usuarios y grupos. Para minimizar la sobrecarga administrativa, la pertenencia a los grupos debe actualizarse automáticamente en función de los títulos de trabajo. 

## Simulación interactiva de laboratorio

Este laboratorio usa una simulación de laboratorio interactiva. La simulación permite hacer clic en escenarios similares a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure.

>**Nota:** Esta simulación se está actualizando. Microsoft Entra ID es el nuevo nombre de Azure Active Directory (Azure AD). 

+ [Administrar identidades de id. de entra](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201). Cree y configure usuarios y asígnelo a grupos. Cree un inquilino de Azure y administre cuentas de invitado. 

## Diagrama de la arquitectura
![Diagrama de la arquitectura del laboratorio 01.](../media/az104-lab01-architecture.png)

## Aptitudes de trabajo

+ Tarea 1: Cree y configure cuentas de usuario.
+ Tarea 2: Cree grupos y agregue miembros.

## Tarea 1: Cree y configure cuentas de usuario

En esta tarea, creará y configurará una cuenta de almacenamiento. Las cuentas de usuario almacenarán datos de usuario como el nombre, el departamento, la ubicación y la información de contacto.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Para continuar con el portal, seleccione **Cancelar** en la pantalla de presentación **Bienvenida a Azure**. 

    >**Nota:** Azure Portal se usa en todos los laboratorios. Si no está familiarizado con Azure, busque y seleccione `Quickstart Center`. Dedique unos minutos a ver el vídeo **Introducción en Azure Portal**. Incluso si ha usado el portal antes, encontrará algunos consejos y trucos para navegar y personalizar la interfaz.
    
1. Busque y seleccione `Microsoft Entra ID`. Microsoft Entra ID es la solución de administración de identidades y acceso basada en la nube de Azure. Dedique unos minutos a familiarizarse con algunas de las características enumeradas en el panel izquierdo. 

1. Seleccione el panel **Vista general** y, a continuación, la pestaña **Administrar inquilinos**. 

    >**¿Sabía que?** Un inquilino es una instancia específica de Microsoft Entra ID que contiene cuentas y grupos. En función de su situación, puede crear más inquilinos y **Cambiar** entre ellos. 

1. Vuelva a la página **Entra ID** presionando hacia atrás en el explorador o seleccionando la opción en el menú de la ruta de navegación.

1. Seleccione **Licencias**. Desde aquí puede comprar una licencia, administrar las licencias que tiene y asignar licencias a usuarios y grupos. Seleccione **Características con licencia** para ver lo que está disponible.
   
### Creación de un nuevo usuario

1. Seleccione **Usuarios**y, después, en la lista desplegable **Nuevo usuario ** seleccione **Crear nuevo usuario**. 

1. Cree un nuevo usuario con la siguiente configuración (deje a otros usuarios con sus valores predeterminados). En la pestaña **Propiedades** observe todos los distintos tipos de información que se pueden incluir en la cuenta de usuario. 

    | Configuración | Value |
    | --- | --- |
    | Nombre principal de usuario | `az104-user1` |
    | Nombre para mostrar | `az104-user1` |
    | Generar automáticamente la contraseña | **checked** |
    | Cuenta habilitada | **checked** |
    | Puesto (pestaña Propiedades) | `IT Lab Administrator` |
    | Departamento (pestaña Propiedades) | `IT` |
    | Ubicación de uso (pestaña Propiedades) | **Estados Unidos** |

1. Una vez que haya terminado de revisar, seleccione **Revisar y crear** y, a continuación, **Crear**.

1. Actualice la página y confirme que se creó el nuevo usuario. 

### Invitación de un usuario externo

1. En la lista desplegable **Nuevo usuario**, seleccione **Invitar a un usuario externo**. 

    | Configuración | Value |
    | --- | --- |
    | Email | dirección de correo electrónico |
    | Nombre para mostrar | su nombre |
    | Enviar mensaje de invitación | **seleccionar la casilla** |
    | Mensaje | `Welcome to Azure and our group project` |

1. Vaya a la pestaña **Propiedades**. Complete la información básica, incluidos estos campos. 

    | Configuración | Valor |
    | --- | --- |
    | Puesto  | `IT Lab Administrator` |
    | department  | `IT` |
    | Ubicación de uso (pestaña Propiedades) | **Estados Unidos** |

1. Seleccione **Revisar e invitar**y, a continuación, **Invitar**.

1. **Actualizar** la página y confirmar que se creó el usuario invitado. Debería recibir el correo electrónico de invitación en breve. 

    >**Nota:** Es poco probable que cree cuentas de usuario individualmente. ¿Sabe cómo planea su organización crear y administrar cuentas de usuario?
    
## Tarea 2: Cree grupos y agregue miembros

En esta tarea, creará una cuenta de grupo. Las cuentas de grupo pueden incluir cuentas de usuario o dispositivos. Estas son dos formas básicas de asignar miembros a grupos: Estática y dinámicamente. Los grupos estáticos requieren que los administradores agreguen y quiten miembros manualmente.  Los grupos dinámicos se actualizan automáticamente en función de las propiedades de una cuenta de usuario o dispositivo. Por ejemplo, el puesto de trabajo. 

1. En Azure Portal, busque y seleccione `Groups`.

1. Dedique un minuto a familiarizarse con la configuración del grupo en el panel izquierdo.

   + **Expiración** le permite configurar una duración de grupo en días. Después de ese momento, el propietario debe renovar el grupo.
   + **La directiva** de nomenclatura le permite configurar palabras bloqueadas y agregar un prefijo o sufijo a los nombres de grupo.

1. En la hoja **Todos los grupos**, seleccione **+ Nuevo grupo** y cree un nuevo grupo.     

    | Configuración | Valor |
    | --- | --- |
    | Tipo de grupo | **Seguridad** |
    | Nombre del grupo | `IT Lab Administrators` |
    | Descripción del grupo | `Administrators that manage the IT lab` |
    | Tipo de pertenencia | **Asignado** |

    >**Nota**: Se requiere una licencia Entra ID Premium P1 o P2 para la pertenencia dinámica. Si hay otros **tipos de pertenencia** disponibles, las opciones se mostrarán en la lista desplegable. 
    
    ![Captura de pantalla de la creación de un grupo asignado.](../media/az104-lab01-create-assigned-group.png)

1. Seleccione **No hay propietarios seleccionados**.

1. En la página **Agregar propietarios**, busque y **seleccione** usted mismo (que se muestra en la esquina superior derecha) como propietario. Observe que puede tener más de un propietario. 

1. Seleccione **No hay miembros seleccionados**.

1. En el panel **Agregar miembros**, busque y **seleccione** el **az104-user1** y el** usuario invitado ** que invitó. Agregue ambos usuarios al grupo. 

1. Seleccione **Crear** para implementar el grupo.

1. **Actualice** la página y asegúrese de que se creó el grupo.

1. Seleccione el nuevo grupo y revise la información**Miembros** y **Propietarios**.

>**Nota:** Puede administrar un gran número de grupos. ¿Tiene su organización un plan para crear grupos y agregar miembros?
   
## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. Tenga en cuenta que esto no quitará ningún usuario o grupo de Entra ID que haya creado. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.
  

## Ampliar el aprendizaje con Copilot

Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.
+ ¿Cuáles son los comandos de Azure PowerShell y la CLI para crear un grupo de seguridad denominado Administradores de TI? Proporcione la página oficial de referencia del comando.  
+ Proporcione una estrategia paso a paso para administrar usuarios y grupos en Microsoft Entra ID.
+ ¿Cuáles son los pasos de Azure Portal para crear usuarios y grupos de forma masiva?
+ Proporcione una tabla de comparación de cuentas de usuario internas y externas de Microsoft Entra ID. 


## Más información con el aprendizaje autodirigido

+ [Comprender Entra de Microsoft ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Compare Microsoft Entra ID con Active Directory DS, obtenga información sobre Microsoft Entra ID P1 y P2 y explore Microsoft Entra Domain Services para administrar aplicaciones y dispositivos unidos a un dominio en la nube.
+ [Crear usuarios y grupos de Azure en el id. de Microsoft Entra](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Crear usuarios en Microsoft Entra ID. Conozca los distintos tipos de grupos. Cree un grupo e incorpore miembros a él. Administre cuentas de invitado de negocio a negocio.
+ [Permitir que los usuarios restablezcan sus contraseñas con el autoservicio de restablecimiento de contraseña de Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Evalúe el autoservicio de restablecimiento de contraseña para permitir que los usuarios de la organización restablezcan sus contraseñas o desbloqueen sus cuentas. Implemente, configure y pruebe el autoservicio de restablecimiento de contraseña.


## Puntos clave

Enhorabuena por completar el laboratorio. Estos son algunos de los principales pasos para este laboratorio:

+ El nuevo inquilino representa a su organización y le ayuda a administrar una instancia específica de Servicios en la nube de Microsoft para los usuarios internos y externos.
+ Microsoft Entra ID tiene cuentas de usuario e invitado. Cada cuenta tiene un nivel de acceso específico para el ámbito de trabajo que se espera que se realice.
+ Los grupos combinan usuarios o dispositivos relacionados. Existen dos tipos de grupos: Seguridad y Microsoft 365.
+ La pertenencia a grupos se puede asignar estática o dinámicamente.
