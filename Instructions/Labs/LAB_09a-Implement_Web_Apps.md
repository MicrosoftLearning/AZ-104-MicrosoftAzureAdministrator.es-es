---
lab:
  title: '09a: Implementación de Web Apps'
  module: Module 09 - Serverless Computing
---

# <a name="lab-09a---implement-web-apps"></a>Laboratorio 09a: Implementación de Web Apps
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

You need to evaluate the use of Azure Web apps for hosting Contoso's web sites, hosted currently in the company's on-premises data centers. The web sites are running on Windows servers using PHP runtime stack. You also need to determine how you can implement DevOps practices by leveraging Azure web apps deployment slots.

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Crear una aplicación web de Azure
+ Tarea 2: Crear una ranura de implementación de ensayo
+ Tarea 3: Configurar las opciones de implementación de la aplicación web
+ Tarea 4: Implementar código en la ranura de implementación de ensayo
+ Tarea 5: Intercambiar los espacios de ensayo
+ Tarea 6: Configurar y probar el escalado automático de la aplicación web de Azure

## <a name="estimated-timing-30-minutes"></a>Tiempo estimado: 30 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab09a.png)

## <a name="instructions"></a>Instructions

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-create-an-azure-web-app"></a>Tarea 1: Crear una aplicación web de Azure

En esta tarea, creará una aplicación web de Azure.

1. Inicie sesión en [**Azure Portal**](http://portal.azure.com).

1. En Azure Portal, busque y seleccione **App Services** y, en la hoja **App Services**, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** de la hoja **Crear aplicación web**, especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | ---|
    | Subscription | nombre de la suscripción de Azure que usa en este laboratorio |
    | Resource group | Nombre de un nuevo grupo de recursos **az104-09a-rg1** |
    | Nombre de aplicación web | Cualquier nombre globalmente único |
    | Publicar | **Código** |
    | Pila en tiempo de ejecución | **PHP 7.4** |
    | Sistema operativo | **Windows** |
    | Region | Nombre de una región de Azure donde puede aprovisionar aplicaciones web de Azure |
    | Plan de App Service | Acepte la configuración predeterminada |

1. Click <bpt id="p1">**</bpt>Review + create<ept id="p1">**</ept>. On the <bpt id="p1">**</bpt>Review + create<ept id="p1">**</ept> tab of the <bpt id="p2">**</bpt>Create Web App<ept id="p2">**</ept> blade, ensure that the validation passed and click <bpt id="p3">**</bpt>Create<ept id="p3">**</ept>.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait until the web app is created before you proceed to the next task. This should take about a minute.

1. En la hoja de implementación, haga clic en **Ir al recurso**.

#### <a name="task-2-create-a-staging-deployment-slot"></a>Tarea 2: Crear una ranura de implementación de ensayo

En esta tarea, creará una ranura de implementación de ensayo.

1. En la hoja de la aplicación web recién implementada, haga clic en el vínculo **URL** para mostrar la página web predeterminada en una nueva pestaña del explorador.

1. Cierre la nueva pestaña del explorador y, de vuelta en Azure Portal, en la sección **Implementación** de la hoja de la aplicación web, haga clic en **Espacios de implementación**.

    >**Nota**: La aplicación web, en este momento, tiene una sola ranura de implementación con la etiqueta **PRODUCTION**.

1. Haga clic en **+ Agregar ranura** y agregue una nueva ranura con la siguiente configuración:

    | Configuración | Valor |
    | --- | ---|
    | Nombre | **staging** |
    | Clonar la configuración de | **No clonar la configuración**|

1. De nuevo en la hoja **Ranuras de implementación** de la aplicación web, haga clic en la entrada que representa el espacio de ensayo recién creado.

    >**Nota**: Se abrirá la hoja que muestra las propiedades del espacio de ensayo.

1. Revise la hoja del espacio de ensayo y tenga en cuenta que su dirección URL difiere de la asignada al espacio de producción.

#### <a name="task-3-configure-web-app-deployment-settings"></a>Tarea 3: Configurar las opciones de implementación de la aplicación web

En esta tarea, configurará las opciones de implementación de la aplicación web.

1. En la hoja de la ranura de implementación de ensayo, en la sección **Implementación**, haga clic en **Centro de implementación** y, a continuación, seleccione la pestaña **Configuración**.

    >**Nota**: Asegúrese de que está en la hoja del espacio de ensayo (en lugar del espacio de producción).
    
1. En la pestaña **Configuración**, en la lista desplegable **Origen**, seleccione **GIT local** y haga clic en el botón **Guardar**.

1. En la hoja **Centro de implementación**, copie la entrada **URL de clonación de GIT** en el Bloc de notas.

    >**Nota**: Necesitará el valor de URL de clonación de GIT en la siguiente tarea de este laboratorio.

1. En la hoja **Centro de implementación**, seleccione la pestaña **Credenciales de GIT o FTPS locales**, en la sección **Ámbito de usuario**, configure las opciones siguientes y haga clic en **Guardar**.

    | Configuración | Valor |
    | --- | ---|
    | Nombre de usuario | Cualquier nombre globalmente único (no debe contener el carácter `@`) |
    | Contraseña | Cualquier contraseña que cumpla los requisitos de complejidad|

    >**Nota**: La contraseña debe tener al menos ocho caracteres y dos de los tres elementos siguientes: letras, números y caracteres no alfanuméricos.

    >**Nota**: Necesitará estas credenciales en la siguiente tarea de este laboratorio.

#### <a name="task-4-deploy-code-to-the-staging-deployment-slot"></a>Tarea 4: Implementar código en la ranura de implementación de ensayo

En esta tarea, implementará código en la ranura de implementación de ensayo.

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**.

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**.

1. En el panel de Cloud Shell, ejecute lo siguiente para clonar el repositorio remoto que contiene el código de la aplicación web.

   ```powershell
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```

1. En el panel de Cloud Shell, ejecute lo siguiente para establecer la ubicación actual en el clon recién creado del repositorio local que contiene el código de la aplicación web de ejemplo.

   ```powershell
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. En el panel de Cloud Shell, ejecute lo siguiente para agregar el GIT remoto (asegúrese de reemplazar los marcadores de posición `[deployment_user_name]` y `[git_clone_url]` por el valor del nombre de usuario de **Credenciales de implementación** y la **URL de clonación de GIT**, respectivamente, que identificó en la tarea anterior):

   ```powershell
   git remote add [deployment_user_name] [git_clone_url]
   ```

    >**Nota**: El valor siguiente `git remote add` no tiene que coincidir con el nombre de usuario de **Credenciales de implementación**, pero debe ser único.

1. En el panel de Cloud Shell, ejecute lo siguiente para insertar el código de la aplicación web de ejemplo del repositorio local en la ranura de implementación de ensayo de la aplicación web de Azure (asegúrese de reemplazar el marcador de posición `[deployment_user_name]` por el valor del nombre de usuario de **Credenciales de implementación**, que identificó en la tarea anterior):

   ```powershell
   git push [deployment_user_name] master
   ```

1. Si se le pide que se autentique, escriba `[deployment_user_name]` y la contraseña correspondiente (que estableció en la tarea anterior).

1. Cierre el panel de Cloud Shell.

1. En la hoja del espacio de ensayo, haga clic en **Información general** y, luego, en el vínculo **URL** para mostrar la página web predeterminada en una nueva pestaña del explorador.

1. Verify that the browser page displays the <bpt id="p1">**</bpt>Hello World!<ept id="p1">**</ept> message and close the new tab.

#### <a name="task-5-swap-the-staging-slots"></a>Tarea 5: Intercambiar los espacios de ensayo

En esta tarea, intercambiará el espacio de ensayo por el espacio de producción.

1. Vuelva a la hoja que muestra el espacio de producción de la aplicación web.

1. En la sección **Implementación**, haga clic en **Ranuras de implementación** y luego en el icono **Intercambiar** de la barra de herramientas.

1. Haga clic en la hoja **Intercambiar**, revise la configuración predeterminada y haga clic en **Intercambiar**.

1. Haga clic en **Información general** en la hoja del espacio de producción de la aplicación web y, luego, en el vínculo **URL** para mostrar la página principal del sitio web en una nueva pestaña del explorador.

1. Verify the default web page has been replaced with the <bpt id="p1">**</bpt>Hello World!<ept id="p1">**</ept> page.

#### <a name="task-6-configure-and-test-autoscaling-of-the-azure-web-app"></a>Tarea 6: Configurar y probar el escalado automático de la aplicación web de Azure

En esta tarea, configurará y probará el escalado automático de la aplicación web de Azure.

1. En la hoja que muestra el espacio de producción de la aplicación web, en la sección **Configuración**, haga clic en **Escalar horizontalmente (plan de App Service)** .

1. Haga clic en **Escalabilidad automática personalizada**.

    >**Nota**: También tiene la opción de escalar manualmente la aplicación web.

1. Deje seleccionada la opción predeterminada **Escalado basado en una métrica** y haga clic en **+ Agregar una regla**.

1. En la hoja **Escalar regla**, configure las opciones siguientes (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- |--- |
    | Origen de métricas | **Recurso actual** |
    | Agregación de tiempo | **Máximo** |
    | Espacio de nombres de métricas | **Métricas estándar de los planes de App Service** |
    | Nombre de métrica | **Porcentaje de CPU** |
    | Operador | **Mayor que** |
    | Umbral de la métrica para desencadenar la acción de escalado | **10** |
    | Duración (en minutos) | **1** |
    | Estadísticas de intervalo de agregación | **Máximo** |
    | Operación | **Aumentar recuento en** |
    | Recuento de instancias | **1** |
    | Tiempo de finalización (minutos) | **5** |

    >**Nota**: Obviamente, estos valores no representan una configuración realista, ya que su propósito es desencadenar el escalado automático lo antes posible, sin un período de espera extendido.

1. Haga clic en **Agregar** y, de nuevo en la hoja de escalado del plan de App Service, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- |--- |
    | Límites mínimos de la instancia | **1** |
    | Límites de instancia, máximo | **2** |
    | Límites de instancia, predeterminado | **1** |

1. Haga clic en **Save**(Guardar).

    >**Nota:** Si se produce un error que indica que el proveedor de recursos “microsoft.insights” no está registrado, ejecute `az provider register --namespace 'Microsoft.Insights'` en Cloud Shell y vuelva a intentar guardar las reglas de escalado automático.

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**.

1. En el panel de Cloud Shell, ejecute lo siguiente para identificar la dirección URL de la aplicación web de Azure.

   ```powershell
   $rgName = 'az104-09a-rg1'

   $webapp = Get-AzWebApp -ResourceGroupName $rgName
   ```

1. En el panel de Cloud Shell, ejecute lo siguiente para iniciar un bucle infinito que envía las solicitudes HTTP a la aplicación web:

   ```powershell
   while ($true) { Invoke-WebRequest -Uri $webapp.DefaultHostName }
   ```

1. Minimice el panel de Cloud Shell (pero no lo cierre) y, en la hoja de la aplicación web, en la sección **Supervisión**, haga clic en **Explorador de procesos**.

    >**Nota**: El Explorador de procesos facilita la supervisión del número de instancias y su uso de recursos.

1. Supervise el uso y el número de instancias durante unos minutos.

    >**Nota**: Es posible que tenga que **actualizar** la página.

1. Una vez que observe que el número de instancias ha aumentado a 2, vuelva a abrir el panel de Cloud Shell y presione **CTRL+C** para finalizar el script.

1. Cierre el panel de Cloud Shell.

#### <a name="clean-up-resources"></a>Limpieza de recursos

>Tiene que evaluar el uso de aplicaciones web de Azure para hospedar sitios web de Contoso, hospedados actualmente en los centros de datos locales de la empresa.

>Los sitios web se ejecutan en servidores Windows mediante la pila en tiempo de ejecución de PHP. 

1. En Azure Portal, abra la sesión de **PowerShell** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*'
   ```

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro -AsJob). Aunque podrá ejecutar otro comando de PowerShell inmediatamente después en la misma sesión de PowerShell, los grupos de recursos tardarán unos minutos en eliminarse.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

+ Creado una aplicación web de Azure
+ Creado una ranura de implementación de ensayo
+ Configurado las opciones de implementación de la aplicación web
+ Implementado código en la ranura de implementación de ensayo
+ Intercambiado las ranuras de ensayo
+ Configurado y probado el escalado automático de la aplicación web de Azure
