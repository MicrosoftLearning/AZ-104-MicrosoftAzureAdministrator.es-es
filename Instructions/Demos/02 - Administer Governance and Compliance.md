---
demo:
  title: "Demostración\_02: Administración de gobernanza y cumplimiento"
  module: Administer Governance and Compliance
---

# 02: Administración de gobernanza y cumplimiento

## Configuración de suscripciones

Esta área no tiene una demostración formal.  

**Referencia**: [Creación de una suscripción de Azure adicional](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Configuración de Azure Policy

En esta demostración, se trabajará con directivas de Azure.

**Referencia**: [Tutorial: Creación de directivas para aplicar el cumplimiento: Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Asignación de una directiva**

1.  Acceda a Azure Portal.

2.  Busque y seleccione  **Directiva**.

3.  Seleccione  **Asignaciones** y después **Asignar directiva**.

5.  Observe el  **Ámbito** que determina en qué recursos o agrupación de recursos se implementa la asignación de directiva.

6.  Seleccione los puntos suspensivos de  **Definición de directiva** para abrir la lista de definiciones disponibles. Tómese un momento para revisar la lista de definiciones de directiva integradas.

7.  Busque y seleccione la directiva  **Ubicaciones permitidas**. Esta directiva permite restringir las ubicaciones que la organización puede especificar al implementar los recursos.

8.  Mueva la pestaña  **Parámetros** y, mediante la lista desplegable, seleccione una o varias ubicaciones permitidas.

9.  Haga clic en  **Revisar y crear** y, a continuación, en **Crear** para crear la directiva.

**Creación y asignación de una definición de iniciativa**

1.  Vuelva a la página de Azure Policy y seleccione  **Definiciones** en Creación.

2.  Seleccione  **Definición de la iniciativa** en la parte superior de la página.

3.  Proporcione un  **Nombre** y una  **Descripción**.

4.  **Cree** una categoría.

5.  En el panel derecho,  **agregue** la directiva  **Ubicaciones permitidas**.

6.  Agregue una directiva adicional de su elección.

7.  **Guarde** los cambios y, después,  **asigne** la definición de la iniciativa a la suscripción.

**Comprobación del cumplimiento**

1.  Vuelva a la página del servicio Azure Policy.

2.  Seleccione  **Cumplimiento**.

3.  Revise el estado de la directiva y la definición.

**Comprobación de tareas de corrección**

1.  Vuelva a la página del servicio Azure Policy.

2.  Seleccione  **Corrección**.

3.  Revise las tareas de corrección que se muestran.

4. Como tiene tiempo, quite la directiva y la iniciativa. 

## Configuración del control de acceso basado en rol

En esta demostración, obtendrá información sobre las asignaciones de roles.

**Referencia**: [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante Azure Portal: Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Referencia**: [Inicio rápido: Comprobación del acceso de un usuario a los recursos de Azure (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Ubicación de la hoja Control de acceso**

1.  Acceda a Azure Portal y seleccione un grupo de recursos.  Tome nota del grupo de recursos que usa.

2.  Seleccione el panel  **Control de acceso (IAM)** .

3.  Este panel estará disponible para muchos recursos diferentes para que pueda controlar los permisos.

**Revisión de permisos de rol**

1.  Seleccione la pestaña  **Roles** (parte superior).

1.  Revise el gran número de roles integrados que están disponibles.

1.  Haga doble clic en un rol y, después, seleccione  **Permisos** (parte superior).

1.  Continúe profundizando en el rol hasta que pueda ver sus acciones  **Lectura, Escritura y Eliminación**.

1.  Vuelva al panel  **Control de acceso (IAM)** .

**Adición de una asignación de roles**

1.  Cree un usuario o seleccione uno existente.

1.  Seleccione  **Agregar asignación de roles** y seleccione un rol. Por ejemplo, *propietario*.

1.  Seleccione  **Comprobar acceso**.

1.  Revise los permisos de usuario.

1.  Tenga en cuenta que puede  **Denegar asignaciones**.
