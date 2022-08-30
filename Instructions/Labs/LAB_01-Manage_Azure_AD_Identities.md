---
lab:
  title: '01: Administración de identidades de Azure Active Directory'
  module: Module 01 - Identity
---

# <a name="lab-01---manage-azure-active-directory-identities"></a>Laboratorio 01: Administración de identidades de Azure Active Directory

# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

In order to allow Contoso users to authenticate by using Azure AD, you have been tasked with provisioning users and group accounts. Membership of the groups should be updated automatically based on the user job titles. You also need to create a test Azure AD tenant with a test user account and grant that account limited permissions to resources in the Contoso Azure subscription.

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Creación y configuración de usuarios de Azure AD
+ Tarea 2: Creación de grupos de Azure AD con pertenencia asignada y dinámica
+ Tarea 3: Creación de un inquilino de Azure Active Directory (AD) (opcional: problema del entorno de laboratorio)
+ Tarea 4: Administración de usuarios invitados de Azure AD (opcional: problema del entorno de laboratorio)

## <a name="estimated-timing-30-minutes"></a>Tiempo estimado: 30 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura
![imagen](../media/lab01.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-create-and-configure-azure-ad-users"></a>Tarea 1: Creación y configuración de usuarios de Azure AD

En esta tarea, creará y configurará usuarios de Azure AD.

>**Nota**: Si ha usado previamente la licencia de prueba para Azure AD Premium en este inquilino de Azure AD, necesitará un nuevo inquilino de Azure AD y realizar la tarea 2 después de la tarea 3 en ese nuevo inquilino de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En Azure Portal, busque y seleccione **Azure Active Directory**.

1. En la hoja Azure Active Directory, desplácese hacia abajo hasta la sección **Administrar**, haga clic en **Configuración de usuario** y revise las opciones de configuración disponibles.

1. En la hoja Azure Active Directory, en la sección **Administrar**, haga clic en **Usuarios** y, a continuación, haga clic en la cuenta de usuario cuya configuración de **Perfil** se debe mostrar. 

1. Haga clic en **Editar**, en la sección **Configuración**, establezca **Ubicación de uso** en **Estados Unidos** y haga clic en **Guardar** para aplicar el cambio.

    >**Nota**: Esto es necesario para asignar una licencia de Azure AD Premium P2 a su cuenta de usuario más adelante en este laboratorio.

1. Vuelva a la hoja **Usuarios: Todos los usuarios** y, a continuación, haga clic en **+ Nuevo usuario**.

1. Cree un nuevo usuario con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre de usuario | **az104-01a-aaduser1** |
    | Name | **az104-01a-aaduser1** |
    | Permitirme crear la contraseña | enabled |
    | Contraseña inicial | **Proporcione una contraseña segura** |
    | Ubicación de uso | **Estados Unidos** |
    | Puesto | **Administrador de la nube** |
    | department | **TI** |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. En la lista de usuarios, haga clic en la cuenta de usuario recién creada para mostrar su hoja.

1. Revise las opciones disponibles en la sección **Administrar** y tenga en cuenta que puede identificar los roles de Azure AD asignados a la cuenta de usuario, así como los permisos de la cuenta de usuario para los recursos de Azure.

1. En la sección **Administrar**, haga clic en **Roles asignados** y, a continuación, haga clic en el botón **+ Agregar asignación** y asigne el rol **Administrador de usuarios** a **az104-01a-aaduser1**.

    >**Nota**: También tiene la opción de asignar roles de Azure AD al aprovisionar un nuevo usuario.

1. Open an <bpt id="p1">**</bpt>InPrivate<ept id="p1">**</ept> browser window and sign in to the <bpt id="p2">[</bpt>Azure portal<ept id="p2">](https://portal.azure.com)</ept> using the newly created user account. When prompted to update the password, change the password to a secure password of your choosing. 

    >**Nota**: En lugar de escribir el nombre de usuario (incluido el nombre de dominio), puede pegar el contenido del Portapapeles.

1. En la ventana **InPrivate** del explorador, en Azure Portal, busque y seleccione **Azure Active Directory**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While this user account can access the Azure Active Directory tenant, it does not have any access to Azure resources. This is expected, since such access would need to be granted explicitly by using Azure Role-Based Access Control. 

1. En la ventana **InPrivate** del explorador, en la hoja Azure AD, desplácese hacia abajo hasta la sección **Administrar**, haga clic en **Configuración de usuario** y tenga en cuenta que no tiene permisos para modificar ninguna opción de configuración.

1. En la ventana **InPrivate** del explorador, en la hoja Azure AD, en la sección **Administrar**, haga clic en **Usuarios** y, a continuación, haga clic en **+ Nuevo usuario**.

1. Cree un nuevo usuario con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre de usuario | **az104-01a-aaduser2** |
    | Name | **az104-01a-aaduser2** |
    | Permitirme crear la contraseña | enabled |
    | Contraseña inicial | **Proporcione una contraseña segura** |
    | Ubicación de uso | **Estados Unidos** |
    | Puesto | **Administrador del sistema** |
    | department | **TI** |

1. Cierre la sesión como usuario az104-01a-aaduser1 desde Azure Portal y cierre la ventana InPrivate del explorador.

#### <a name="task-2-create-azure-ad-groups-with-assigned-and-dynamic-membership"></a>Tarea 2: Creación de grupos de Azure AD con pertenencia asignada y dinámica

En esta tarea, creará grupos de Azure Active Directory con pertenencia asignada y dinámica.

1. De nuevo en Azure Portal en el que ha iniciado sesión con su **cuenta de usuario**, vuelva a la hoja **Información general** del inquilino de Azure AD y, en la sección **Administrar**, haga clic en **Licencias**.

    >**Nota**: Las licencias de Azure AD Premium P1 o P2 son necesarias para implementar grupos dinámicos.

1. En la sección **Administrar**, haga clic en **Todos los productos**.

1. Haga clic en **+ Probar o comprar** y active la evaluación gratuita de Azure AD Premium P2.

1. Actualice la ventana del explorador para comprobar que la activación se ha realizado correctamente. 

 ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: It can take up to 10 minutes for the licenses to activate. Continue refreshing the page until it appears. Do not proceed until the licenses have been activated.

1. En la hoja **Licencias: Todos los productos**, seleccione la entrada **Azure Active Directory Premium P2** y asigne todas las opciones de licencia de Azure AD Premium P2 a su cuenta de usuario y a las dos cuentas de usuario recién creadas.

1. En Azure Portal, vuelva a la hoja del inquilino de Azure AD y haga clic en **Grupos**.

1. Use el botón **+ Nuevo grupo** para crear un nuevo grupo con la siguiente configuración:

    | Configuración | Valor |
    | --- | --- |
    | Tipo de grupo | **Seguridad** |
    | Nombre del grupo | **Administradores de la nube de TI** |
    | Descripción del grupo | **Administradores de la nube de TI de Contoso** |
    | Tipo de pertenencia | **Usuario dinámico** |

    >**Nota**: Si la lista desplegable **Tipo de pertenencia** está atenuada, espere unos minutos y actualice la página del explorador.

1. Haga clic en **Agregar una consulta dinámica**.

1. En la pestaña **Configurar reglas** de la hoja **Reglas de pertenencia dinámica**, cree una nueva regla con la siguiente sintaxis:

    | Configuración | Valor |
    | --- | --- |
    | Propiedad | **jobTitle** |
    | Operador | **Es igual a** |
    | Value | **Administrador de la nube** |

1. Para permitir que los usuarios de Contoso se autentiquen mediante Azure AD, se le ha encargado el aprovisionamiento de usuarios y cuentas de grupo. 

1. De nuevo en la hoja **Grupos: Todos los grupos** del inquilino de Azure AD, haga clic en el botón **+ Nuevo grupo** y cree un grupo con la siguiente configuración:

    | Configuración | Valor |
    | --- | --- |
    | Tipo de grupo | **Seguridad** |
    | Nombre del grupo | **Administradores del sistema de TI** |
    | Descripción del grupo | **Administradores del sistema de TI de Contoso** |
    | Tipo de pertenencia | **Usuario dinámico** |

1. Haga clic en **Agregar una consulta dinámica**.

1. En la pestaña **Configurar reglas** de la hoja **Reglas de pertenencia dinámica**, cree una nueva regla con la siguiente sintaxis:

    | Configuración | Valor |
    | --- | --- |
    | Propiedad | **jobTitle** |
    | Operador | **Es igual a** |
    | Value | **Administrador del sistema** |

1. La pertenencia a los grupos debe actualizarse automáticamente en función de los puestos de trabajo del usuario. 

1. De nuevo en la hoja **Grupos: Todos los grupos** del inquilino de Azure AD, haga clic en el botón **+ Nuevo grupo** y cree un grupo con la siguiente configuración:

    | Configuración | Valor |
    | --- | --- |
    | Tipo de grupo | **Seguridad** |
    | Nombre del grupo | **Administradores del laboratorio de TI** |
    | Descripción del grupo | **Administradores del laboratorio de TI de Contoso** |
    | Tipo de pertenencia | **Asignado** |
    
1. Haga clic en **No hay miembros seleccionados**.

1. En la hoja **Agregar miembros**, busque y seleccione los grupos **Administradores de la nube de TI** y **Administradores del sistema de TI** y, de nuevo en la hoja **Nuevo grupo**, haga clic en **Crear**.

1. También debe crear un inquilino de prueba de Azure AD con una cuenta de usuario de prueba y conceder permisos limitados a esa cuenta para los recursos de la suscripción de Azure de Contoso.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: You might experience delays with updates of the dynamic membership groups. To expedite the update, navigate to the group blade, display its <bpt id="p1">**</bpt>Dynamic membership rules<ept id="p1">**</ept> blade, <bpt id="p2">**</bpt>Edit<ept id="p2">**</ept> the rule listed in the <bpt id="p3">**</bpt>Rule syntax<ept id="p3">**</ept> textbox by adding a whitespace at the end, and <bpt id="p4">**</bpt>Save<ept id="p4">**</ept> the change.

1. Navigate back to the <bpt id="p1">**</bpt>Groups - All groups<ept id="p1">**</ept> blade, click the entry representing the <bpt id="p2">**</bpt>IT System Administrators<ept id="p2">**</ept> group and, on then display its <bpt id="p3">**</bpt>Members<ept id="p3">**</ept> blade. Verify that the <bpt id="p1">**</bpt>az104-01a-aaduser2<ept id="p1">**</ept> appears in the list of group members.

#### <a name="task-3-create-an-azure-active-directory-ad-tenant-optional---lab-environment-issue"></a>Tarea 3: Creación de un inquilino de Azure Active Directory (AD) (opcional: problema del entorno de laboratorio)

En esta tarea, creará un nuevo inquilino de Azure AD.

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: There is a known issue with the Captcha verification in the lab environment. If you experience this issue, please skip both this task and the next. We are working on a solution.

1. En Azure Portal, busque y seleccione **Azure Active Directory**.

1. Haga clic en **Administrar inquilinos**. En la siguiente pantalla, haga clic en **+ Crear** y especifique la siguiente configuración:

    | Configuración | Value |
    | --- | --- |
    | Tipo de directorio | **Azure Active Directory** |
    
1. Haga clic en **Siguiente:Configuración**.

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la organización | **Laboratorio de Contoso** |
    | Nombre de dominio inicial | cualquier nombre DNS válido compuesto de letras minúsculas y dígitos, y que empiece por una letra | 
    | País/región | **Estados Unidos** |

   > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Initial domain name<ept id="p2">**</ept> should not be a legitimate name that potentially matches your organization or another. The green check mark in the <bpt id="p1">**</bpt>Initial domain name<ept id="p1">**</ept> text box will indicate that the domain name you typed in is valid and unique.

1. Haga clic en **Revisar y crear** y, a continuación, en **Crear**.

1. Muestre la hoja del inquilino de Azure AD recién creado mediante el vínculo **Click here to navigate to your new tenant: Contoso Lab** (Haga clic aquí para ir al nuevo inquilino: Contoso Lab) o el botón **Directorio + suscripción** (directamente a la derecha del botón de Cloud Shell) de la barra de herramientas de Azure Portal.

#### <a name="task-4-manage-azure-ad-guest-users"></a>Tarea 4: Administración de usuarios invitados de Azure AD

En esta tarea, creará usuarios invitados de Azure AD y les concederá acceso a los recursos de una suscripción de Azure.

1. En la instancia de Azure Portal que muestra el inquilino de Azure AD del Laboratorio de Contoso, en la sección **Administrar**, haga clic en **Usuarios** y, a continuación, en **+ Nuevo usuario**.

1. Cree un nuevo usuario con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre de usuario | **az104-01b-aaduser1** |
    | Name | **az104-01b-aaduser1** |
    | Permitirme crear la contraseña | enabled |
    | Contraseña inicial | **Proporcione una contraseña segura** |
    | Puesto | **Administrador del sistema** |
    | department | **TI** |

1. Haga clic en el perfil recién creado.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. Cambie de nuevo al inquilino de Azure AD predeterminado mediante el botón **Directorio + suscripción** (directamente a la derecha del botón de Cloud Shell) de la barra de herramientas de Azure Portal.

1. Vuelva a la hoja **Usuarios: Todos los usuarios** y, a continuación, haga clic en **+ Nuevo usuario invitado**.

1. Invite un nuevo usuario invitado con las siguientes opciones de configuración (deje las demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre | **az104-01b-aaduser1** |
    | Dirección de correo electrónico | el nombre principal de usuario que copió anteriormente en esta tarea |
    | Ubicación de uso | **Estados Unidos** |
    | Puesto | **Administrador del laboratorio** |
    | department | **TI** |

1. Haga clic en **Invitar**. 

1. De nuevo en la hoja **Usuarios: Todos los usuarios**, haga clic en la entrada que representa la cuenta de usuario invitado recién creada.

1. En la hoja **az104-01b-aaduser1: Perfil**, haga clic en **Grupos**.

1. Haga clic en **+ Agregar pertenencia** y agregue la cuenta de usuario invitado al grupo **Administradores del laboratorio de TI**.


#### <a name="task-5-clean-up-resources"></a>Tarea 5: Limpieza de recursos

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. While, in this case, there are no additional charges associated with Azure Active Directory tenants and their objects, you might want to consider removing the user accounts, the group accounts, and the Azure Active Directory tenant you created in this lab.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. In the <bpt id="p1">**</bpt>Azure Portal<ept id="p1">**</ept> search for <bpt id="p2">**</bpt>Azure Active Directory<ept id="p2">**</ept> in the search bar. Within <bpt id="p1">**</bpt>Azure Active Directory<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>Licenses<ept id="p3">**</ept>. Once at <bpt id="p1">**</bpt>Licenses<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>All Products<ept id="p3">**</ept> and then select <bpt id="p4">**</bpt>Azure Active Directory Premium P2<ept id="p4">**</ept> item in the list. Proceed by then selecting <bpt id="p1">**</bpt>Licensed Users<ept id="p1">**</ept>. Select the user accounts <bpt id="p1">**</bpt>az104-01a-aaduser1<ept id="p1">**</ept> and <bpt id="p2">**</bpt>az104-01a-aaduser2<ept id="p2">**</ept> to which you assigned licenses in this lab, click <bpt id="p3">**</bpt>Remove license<ept id="p3">**</ept>, and, when prompted to confirm, click <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept>.

1. En Azure Portal, vaya a la hoja **Usuarios: Todos los usuarios**, haga clic en la entrada que representa la cuenta de usuario invitado **az104-01b-aaduser1**, en la hoja **az104-01b-aaduser1: Perfil** haga clic en **Eliminar** y, cuando se le pida confirmación, haga clic en **Aceptar**.

1. Repita la misma secuencia de pasos para eliminar las cuentas de usuario restantes que creó en este laboratorio.

1. Vaya a la hoja **Grupos - Todos los grupos**, seleccione los grupos que creó en este laboratorio, haga clic en **Eliminar** y, cuando se le pida confirmación, haga clic en **Aceptar**.

1. En Azure Portal, muestra la hoja del inquilino de Azure AD del Laboratorio de Contoso mediante el botón **Directorio + suscripción** (directamente a la derecha del botón de Cloud Shell) de la barra de herramientas de Azure Portal.

1. Vaya a la hoja **Usuarios: Todos los usuarios**, haga clic en la entrada que representa la cuenta de usuario **az104-01b-aaduser1**, en la hoja **az104-01b-aaduser1: Perfil** haga clic en **Eliminar** y, cuando se le pida confirmación, haga clic en **Aceptar**.

1. Navigate to the <bpt id="p1">**</bpt>Contoso Lab - Overview<ept id="p1">**</ept> blade of the Contoso Lab Azure AD tenant, click <bpt id="p2">**</bpt>Manage tenants<ept id="p2">**</ept> and then on the next screen, select the box next to <bpt id="p3">**</bpt>Contoso Lab<ept id="p3">**</ept>, click <bpt id="p4">**</bpt>Delete<ept id="p4">**</ept>, on the <bpt id="p5">**</bpt>Delete tenant 'Contoso Labs'?<ept id="p5">**</ept> blade, click the <bpt id="p1">**</bpt>Get permission to delete Azure resources<ept id="p1">**</ept> link, on the <bpt id="p2">**</bpt>Properties<ept id="p2">**</ept> blade of Azure Active Directory, set <bpt id="p3">**</bpt>Access management for Azure resources<ept id="p3">**</ept> to <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept> and click <bpt id="p5">**</bpt>Save<ept id="p5">**</ept>.

1. Vuelva a la hoja **Eliminar inquilino “Contoso Lab”** , seleccione **Actualizar** y haga clic en **Eliminar**.

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: If a tenant has a trial license, then you would have to wait for the trial license expiration before you could delete the tenant. This would not incur any additional cost.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Creado y configurado usuarios de Azure AD
- Creado grupos de Azure AD con pertenencia asignada y dinámica
- Creado un inquilino de Azure Active Directory (AD)
- Administrado usuarios invitados en Azure AD 
