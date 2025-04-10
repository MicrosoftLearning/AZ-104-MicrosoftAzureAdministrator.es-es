---
lab:
  title: 'Laboratorio 09a: Implementación de aplicaciones web'
  module: Administer PaaS Compute Options
---

# Laboratorio 09a: Implementación de Web Apps


## Introducción al laboratorio

En este laboratorio, obtendrá información sobre las aplicaciones web de Azure. Aprenderá a configurar una aplicación web para mostrar una aplicación Hola mundo en un repositorio externo de GitHub. Aprenderá a crear un espacio de ensayo y a intercambiarlo con el espacio de producción. También obtendrá información sobre el escalado automático para dar cabida a los cambios de demanda.

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para Este de EE. UU.

## Tiempo estimado: 20 minutos

## Escenario del laboratorio

Su organización está interesada en las aplicaciones web de Azure para hospedar los sitios web de su empresa. Actualmente, estos sitios web se hospedan en centros de datos locales. Los sitios web se ejecutan en servidores Windows con la pila de runtime de PHP. El hardware está cerca del final de la vida útil y pronto tendrá que reemplazarse. Su organización quiere evitar nuevos costes de hardware usando Azure para hospedar los sitios web. 

## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure.

+ [Creación de una aplicación web](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202). Cree una aplicación web que ejecute un contenedor Docker.
    
+ [Implementación de aplicaciones web de Azure](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013). Cree una aplicación web de Azure, administre la implementación y escale la aplicación. 

## Diagrama de la arquitectura

![Diagrama de las tareas.](../media/az104-lab09a-architecture.png)

## Aptitudes de trabajo

+ Tarea 1: Cree y configure una aplicación web de Azure.
+ Tarea 2: Cree y configure una ranura de implementación.
+ Tarea 3: configuración de las opciones de implementación de la aplicación web.
+ Tarea 4: Intercambie ranuras de implementación.
+ Tarea 5: Configure y pruebe el escalado automático de la aplicación web de Azure.

## Tarea 1: Creación y configuración de una aplicación web de Azure

En esta tarea, creará una aplicación web de Azure. Azure App Services es una solución de plataforma como servicio (PAAS) para aplicaciones web, móviles y otras basadas en web. Las aplicaciones web de Azure forman parte de las instancias de Azure App Services que hospedan la mayoría de los entornos en runtime, como PHP, Java y .NET. El plan de App Service que seleccione determinará el proceso, el almacenamiento y las características de la aplicación web. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `App services`.

1. Seleccione **+ Crear**, en el menú desplegable **Aplicación web**. Observe las otras opciones. 

1. En la pestaña **Aspectos básicos** de la hoja **Crear aplicación web**, especifique las siguientes opciones de configuración (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | ---|
    | Subscription | su suscripción de Azure |
    | Resource group | `az104-rg9` (Si es necesario, seleccione **Crear nuevo**) |
    | Nombre de aplicación web | Cualquier nombre globalmente único |
    | Publicar | **Código** |
    | Pila en tiempo de ejecución | **PHP 8.2** |
    | Sistema operativo | **Linux** |
    | Region | **Este de EE. UU.** |
    | Panes de tarifa | **Premium V3 P1V3** |
    | Redundancia de zona | aceptar los valores predeterminados |

 1. Haz clic en **Revisar y crear** y, a continuación, en **Crear**.

    >**Nota**: espera hasta que se cree la aplicación web antes de continuar con la tarea siguiente. Este proceso tardará aproximadamente un minuto.
    
    >**Nota**: si se produce un error en la implementación, cambia a otra región e inténtalo de nuevo. Esto se debe a cuotas en diferentes regiones.  

1. Una vez que la implementación se haya completado, selecciona **Ir al recurso**.

## Tarea 2: Creación y configuración de una ranura de implementación

En esta tarea, crearás un espacio de implementación de ensayo. Los espacios de implementación permiten realizar pruebas antes de que la aplicación esté disponible para el público (o para los usuarios finales). Después de realizar las pruebas, podrás intercambiar el espacio del desarrollo o pasar de Almacenamiento provisional a Producción. Muchas organizaciones usan espacios para realizar pruebas de preproducción. Además, muchas organizaciones ejecutan varios espacios para cada aplicación (por ejemplo, desarrollo, control de calidad, prueba y producción).

1. En la hoja de la aplicación web recién implementada, haz clic en el vínculo **Dominio predeterminado** para mostrar la página web predeterminada en una nueva pestaña del explorador.

1. Cierra la nueva pestaña del explorador y, de vuelta en Azure Portal, en la sección **Implementación** de la hoja de la aplicación web, haz clic en **Ranuras de implementación**.

1. Haz clic en **Agregar espacio** y agrega un nuevo espacio con la siguiente configuración:

    | Configuración | Valor |
    | --- | ---|
    | Nombre | `staging` |
    | Clonar la configuración de | **No clonar la configuración**|

1. Selecciona **Agregar** para crear el espacio.

1. Actualiza la página para ver los espacios de producción y ensayo. 

1. Selecciona la entrada que representa el espacio de ensayo recién creado.

    >**Nota**: Se abrirá la hoja que muestra las propiedades del espacio de ensayo.

1. Revisa la hoja del espacio de ensayo y ten en cuenta que su dirección URL difiere de la asignada al espacio de producción.

## Tarea 3: Configuración de las opciones de implementación de la aplicación web

En esta tarea, configurarás las opciones de implementación de la aplicación web. La configuración de implementación permite la implementación continua. Esto garantiza que el servicio de aplicaciones tenga la versión más reciente de la aplicación.

1. En el espacio de ensayo, selecciona **Centro de implementación** y, a continuación, selecciona **Configuración**.

    >**Nota:** asegúrate de que está en la hoja del espacio de ensayo (y no en el espacio de producción).
    
1. En la lista desplegable **Origen**, selecciona **Git externo**. Observa las otras opciones. 

1. En el campo de repositorio, escribe `https://github.com/Azure-Samples/php-docs-hello-world`.

1. En el campo de rama, escribe `master`.

1. Selecciona **Guardar**.

1. En el espacio de ensayo, selecciona **Información general**.

1. Selecciona el vínculo **Dominio predeterminado** y abre la dirección URL en una nueva pestaña. 

1. Comprueba que el espacio de ensayo muestre **Hola mundo**. 

>**Nota:** la implementación puede tardar un momento. Asegúrate de **actualizar** la página de la aplicación.

## Tarea 4: Intercambio de espacios de implementación

En esta tarea, intercambiarás el espacio de ensayo por el espacio de producción. El intercambio de un espacio te permite usar el código que has probado en el espacio de ensayo y moverlo a producción. Azure Portal también te pedirá si necesitas mover otras configuraciones de aplicación que hayas personalizado para el espacio. Intercambiar espacios es una tarea común para los equipos de aplicaciones y los equipos de soporte técnico de aplicaciones, especialmente para aquellos que implementan actualizaciones de aplicaciones rutinarias y correcciones de errores.

1. Vuelve a la hoja **Espacios de implementación** y selecciona **Intercambiar**.

1. Usa los valores predeterminados y haz clic en **Iniciar intercambio**. Espera a la notificación de que el intercambio ha finalizado.

1. Vuelve a la página principal del portal. Debes tener tanto una aplicación web de producción como el espacio de ensayo.

1. En la hoja**Información general** de App Service Web Apps, selecciona el vínculo **Dominio predeterminado** para mostrar la página principal del sitio web.

1. Comprueba que la página web de producción muestre la página **Hola mundo** .

    >**Nota:** copia la dirección **URL** de dominio predeterminada que necesitarás para las pruebas de carga en la siguiente tarea. 

## Tarea 5: Configuración y prueba del escalado automático de la aplicación web de Azure

En esta tarea, configurarás el escalado automático de la aplicación web de Azure. El escalado automático permite mantener un rendimiento óptimo de la aplicación web cuando aumenta el tráfico a esta. Para determinar cuándo debes escalar la aplicación, puedes supervisar métricas como el uso de CPU, la memoria o el ancho de banda.

1. En la sección **Configuración**, selecciona **Escalar horizontalmente (plan de App Service)**.

    >**Nota:** asegúrate de que estás trabajando en el espacio de producción y no en el de ensayo.  

1. En la sección **Escalado**, selecciona **Automático**. Observa la opción **Basado en reglas**. El escalado basado en reglas se puede configurar para diferentes métricas de aplicación. 

1. En el campo **Ráfaga máxima**, selecciona **2**.

    ![Captura de pantalla de la página de escalabilidad automática.](../media/az104-lab09a-autoscale.png)

1. Selecciona **Guardar**.

1. Selecciona **Diagnosticar y solucionar problemas** (panel izquierdo).

1. En el cuadro **Prueba de carga de la aplicación**, selecciona **Crear prueba de carga**.

    + Selecciona **+ Crear** y asigna un nombre a tu **prueba de carga**.  El nombre debe ser único.
    + Selecciona **Revisar y crear** y, a continuación, **Crear**.

1. Espera a que se cree la prueba de carga y selecciona **Ir al recurso**.

1. Desde **Información general** | **Crear agregando solicitudes HTTP**, selecciona **Crear**.

1. En la pestaña **Plan de prueba**, haz clic en **Agregar solicitud**. En el **campo Dirección URL**, pega la dirección URL de **Dominio predeterminado**. Asegúrate de que tengas el formato correcto y de que comience por **https://**. Haz clic en **Agregar** para guardar los cambios. 

1. Selecciona **Revisar y crear** y **Crear**.

    >**Nota:** la creación de la prueba puede tardar unos minutos. Visualiza las notificaciones.

1. Ve a la prueba (aparece en la página principal). 

1. Actualiza y revisa las métricas dinámicas, incluidos los **usuarios virtuales**, el **tiempo de respuesta** y **las solicitudes por segundo**.

1. Selecciona **Detener** para completar la serie de pruebas. No es necesario esperar a que finalice la prueba. 

## Limpieza de los recursos

Si utilizas **tu propia suscripción**, dedica un minuto a eliminar los recursos del laboratorio. De esta forma estarás seguro de que los recursos se liberan y de que se minimiza el coste. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, selecciona el grupo de recursos, selecciona **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haz clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarte a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesitas más información. Abre un explorador Edge y elige Copilot (superior derecha) o ve a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ Resumir los pasos para crear y configurar una aplicación web de Azure.
+ ¿Cuáles son las formas en que puedo escalar una aplicación web de Azure?

## Más información con el aprendizaje autodirigido

+ [Ensayo de la implementación de una aplicación web para pruebas y reversión mediante ranuras de implementación de App Service](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/). Use las ranuras de implementación para simplificar la implementación y revertir una aplicación web en Azure App Service.
+ [Escale una aplicación web de App Service para satisfacer la demanda de forma eficaz con el escalado vertical y horizontal de App Service](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/). Ante períodos de mayor actividad, aumente de manera incremental los recursos disponibles y, después, cuando disminuya la actividad, redúzcalos para reducir costos.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Azure App Services le permite compilar, implementar y escalar aplicaciones web rápidamente.
+ App Service incluye compatibilidad con muchos entornos de desarrollador, como ASP.NET, Java, PHP y Python.
+ Las ranuras de implementación permiten crear entornos independientes para implementar y probar la aplicación web.
+ Puede escalar de forma manual o automática una aplicación web para controlar la demanda adicional.
+ Hay disponible una amplia variedad de herramientas de diagnóstico y pruebas. 
