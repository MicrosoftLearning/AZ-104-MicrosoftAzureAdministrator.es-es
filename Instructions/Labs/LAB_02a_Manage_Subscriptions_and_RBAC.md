---
lab:
  title: '02a: Administrar suscripciones y RBAC'
  module: Module 02 - Governance and Compliance
ms.openlocfilehash: 657e62b4bc0a34da0748c95922d2e3f4868a21c3
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625564"
---
# <a name="lab-02a---manage-subscriptions-and-rbac"></a>Laboratorio 02a: Administrar suscripciones y RBAC
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-requirements"></a>Requisitos del laboratorio:

Este laboratorio requiere permisos para crear usuarios de Azure Active Directory (Azure AD), crear roles personalizados de control de acceso basado en roles (RBAC) de Azure y asignar estos roles a usuarios de Azure AD. Puede que no todos los hospedadores de laboratorio proporcionen esta funcionalidad. Pregunte al instructor acerca de la disponibilidad de este laboratorio.

## <a name="lab-scenario"></a>Escenario del laboratorio

Para mejorar la administración de recursos de Azure en Contoso, se le ha encargado implementar la funcionalidad siguiente:

- crear un grupo de administración que incluya todas las suscripciones de Azure de Contoso;

- conceder permisos para enviar las solicitudes de soporte técnico para todas las suscripciones del grupo de administración a un usuario designado de Azure Active Directory. Los permisos de ese usuario solo deben limitarse a: 

    - crear solicitud de soporte técnico;
    - ver grupos de recursos. 

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Implementar grupos de administración
+ Tarea 2: Crear roles RBAC personalizados 
+ Tarea 3: Asignar roles RBAC


## <a name="estimated-timing-30-minutes"></a>Tiempo estimado: 30 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab02a.png)


## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-implement-management-groups"></a>Tarea 1: Implementar grupos de administración

En esta tarea, creará y configurará grupos de administración. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque y seleccione **Grupos de administración** para ir a la hoja **Grupos de administración**.

1. Revise los mensajes de la parte superior de la hoja **Grupos de administración**. Si ve el mensaje que indica **Está registrado como administrador de directorio, pero no tiene los permisos necesarios para acceder al grupo de administración raíz. Haga clic para obtener más información**, siga la secuencia de pasos a continuación:

    1. En Azure Portal, busque y seleccione **Azure Active Directory**.
    
    1.  En la hoja que muestra las propiedades del inquilino de Azure Active Directory, en la sección **Administrar** del menú vertical, seleccione **Propiedades**.
    
    1.  En la hoja **Propiedades** de su inquilino de Azure Active Directory, en la sección **Administración de acceso para los recursos de Azure**, seleccione **Sí** y, a continuación, seleccione **Guardar**.
    
    1.  Vuelva a la hoja **Grupos de administración** y seleccione **Actualizar**.

1. En la hoja **Grupos de administración**, haga clic en **+ Crear**.

    >**Nota**: Si no ha creado anteriormente grupos de administración, seleccione **Empezar a usar grupos de administración**.

1. Cree un grupo de administración con la siguiente configuración:

    | Configuración | Value |
    | --- | --- |
    | Id. de grupo de administración | **az104-02-mg1** |
    | Nombre para mostrar del grupo de administración | **az104-02-mg1** |

1. En la lista de grupos de administración, haga clic en la entrada que representa el grupo de administración recién creado.

1. En la hoja **az104-02-mg1**, haga clic en **Suscripciones**. 

1. En la hoja **az104-02-mg1 \| Suscripciones**, haga clic en **+ Agregar**, en la hoja **Agregar suscripción**, en la lista desplegable **Suscripción**, seleccione la suscripción que está usando en este laboratorio y haga clic en **Guardar**.

    >**Nota:** En la hoja **az104-02-mg1 \| Suscripciones**, copie en el Portapapeles el identificador de la suscripción de Azure. Lo necesitará en la próxima tarea.

#### <a name="task-2-create-custom-rbac-roles"></a>Tarea 2: Crear roles RBAC personalizados

En esta tarea, creará una definición de un rol RBAC personalizado.

1. En el equipo de laboratorio, abra el archivo **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** en el Bloc de notas y revise el contenido:

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```

1. Reemplace el marcador de posición `SUBSCRIPTION_ID` del archivo JSON por el identificador de la suscripción que copió en el Portapapeles y guarde el cambio.

1. Haga clic en el icono de la barra de herramientas inmediatamente a la derecha del cuadro de texto de búsqueda en Azure Portal para abrir el panel de **Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**. 

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**. 

1. En la barra de herramientas del panel de Cloud Shell, haga clic en el icono **Cargar/Descargar archivos**, haga clic en **Cargar** en el menú desplegable y cargue el archivo **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** en el directorio principal de Cloud Shell.

1. En el panel de Cloud Shell, ejecute lo siguiente para crear la definición de rol personalizado:

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. Cierre el panel de Cloud Shell.

#### <a name="task-3-assign-rbac-roles"></a>Tarea 3: Asignar roles RBAC

En esta tarea, creará un usuario de Azure Active Directory, asignará a ese usuario el rol RBAC que creó en la tarea anterior, y comprobará que el usuario puede realizar la tarea especificada en la definición del rol RBAC.

1. En Azure Portal, busque y seleccione **Azure Active Directory**, en la hoja Azure Active Directory, haga clic en **Usuarios** y, a continuación, haga clic en **+ Nuevo usuario**.

1. Cree un nuevo usuario con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre de usuario | **az104-02-aaduser1**|
    | Name | **az104-02-aaduser1**|
    | Permitirme crear la contraseña | enabled |
    | Contraseña inicial | **Pa55w.rd1234** |

    >**Nota**: **Copie en el Portapapeles** el **Nombre de usuario** completo. Lo necesitará más adelante en este laboratorio.

1. En Azure Portal, vuelva al grupo de administración **az104-02-mg1** y muestre sus **detalles**.

1. Haga clic en **Control de acceso (IAM)** , haga clic en **+ Agregar** seguido de **Asignación de roles** y asigne el rol **Colaborador de solicitud de soporte técnico (personalizado)** a la cuenta de usuario recién creada.

1. Abra una ventana **InPrivate** del explorador e inicie sesión en [Azure Portal](https://portal.azure.com) con la cuenta de usuario recién creada. Cuando se le pida que actualice la contraseña, cambie la contraseña del usuario.

    >**Nota**: En lugar de escribir el nombre de usuario, puede pegar el contenido del Portapapeles.

1. En la ventana del explorador **InPrivate**, en Azure Portal, busque y seleccione **Grupos de recursos** para comprobar que el usuario az104-02-aaduser1 puede ver todos los grupos de recursos.

1. En la ventana del explorador **InPrivate**, en Azure Portal, busque y seleccione **Todos los recursos** para comprobar que el usuario az104-02-aaduser1 no puede ver ningún recurso.

1. En la ventana del explorador **InPrivate**, en Azure Portal, busque y seleccione **Ayuda y soporte técnico** y, luego, haga clic en **Crear solicitud de soporte técnico**. 

1. En la ventana del explorador **InPrivate**, en la pestaña **Descripción/Resumen del problema** de la hoja **Ayuda y soporte técnico: Nueva solicitud de soporte técnico**, escriba **Límites de servicio y suscripción** en el campo Resumen y seleccione el tipo de problema **Límites de servicio y suscripción (cuotas)** . Tenga en cuenta que la suscripción que está usando en este laboratorio aparece en la lista desplegable **Suscripción**.

    >**Nota**: La presencia de la suscripción que está usando en este laboratorio en la lista desplegable **Suscripción** indica que la cuenta que está usando tiene los permisos necesarios para crear la solicitud de soporte técnico específica de la suscripción.

    >**Nota**: Si no ve la opción **Límites de servicio y suscripción (cuotas)** , cierre sesión en Azure Portal y vuelva a iniciar sesión.

1. No continúe con la creación de la solicitud de soporte técnico. En su lugar, cierre la sesión como usuario az104-02-aaduser1 de Azure Portal y cierre la ventana InPrivate del explorador.

#### <a name="clean-up-resources"></a>Limpieza de recursos

   >**Nota**: No olvide quitar los recursos de Azure recién creados que ya no use. 

   >**Nota**: La eliminación de recursos sin usar garantiza que no aparezcan cargos inesperados, aunque los recursos creados en este laboratorio no incurren en costos adicionales.

1. En Azure Portal, busque y seleccione **Azure Active Directory**, en la hoja Azure Active Directory, haga clic en **Usuarios**.

1. En la hoja **Usuarios: Todos los usuarios**, haga clic en **az104-02-aaduser1**.

1. En la hoja **az104-02-aaduser1: Perfil**, copie el valor del atributo **Id. de objeto**.

1. En Azure Portal, inicie una sesión de **PowerShell** en **Cloud Shell**.

1. En el panel de Cloud Shell, ejecute lo siguiente para quitar la asignación de la definición de rol personalizado (reemplace el marcador de posición `[object_ID]` por el valor del atributo **Id. de objeto** de la cuenta de usuario **az104-02-aaduser1** de Azure Active Directory que copió anteriormente en esta tarea):

   ```powershell
   $scope = (Get-AzRoleAssignment -RoleDefinitionName 'Support Request Contributor (Custom)').Scope

   Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. En el panel de Cloud Shell, ejecute lo siguiente para quitar la definición de rol personalizado:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. En Azure Portal, vuelva a la hoja **Usuarios: Todos los usuarios** de **Azure Active Directory** y elimine la cuenta de usuario **az104-02-aaduser1**.

1. En Azure Portal, vuelva a la hoja **Grupos de administración**. 

1. En la hoja **Grupos de administración**, seleccione el icono de **puntos suspensivos** situado junto a la suscripción en el grupo de administración **az104-02-mg1** y seleccione **Mover** para mover la suscripción al **grupo de administración raíz del inquilino**.

   >**Nota**: Es probable que el grupo de administración de destino sea el **grupo de administración raíz del inquilino**, a menos que haya creado una jerarquía personalizada de grupos de administración antes de ejecutar este laboratorio.
   
1. Seleccione **Actualizar** para comprobar que la suscripción se ha movido correctamente al **grupo de administración raíz del inquilino**.

1. Vuelva a la hoja **Grupos de administración**, haga clic con el botón derecho en el icono de **puntos suspensivos** situado a la derecha del grupo de administración **az104-02-mg1** y haga clic en **Eliminar**.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Implementado grupos de administración
- Creado roles RBAC personalizados 
- Asignado roles RBAC
