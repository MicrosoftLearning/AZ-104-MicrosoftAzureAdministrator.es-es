---
lab:
  title: "Laboratorio\_09c: Implementación de Azure Container Apps"
  module: Administer PaaS Compute Options
---

# Laboratorio 09c: Implementación de Azure Container Apps
# Manual de laboratorio para alumnos

## Escenario del laboratorio
Azure Container Apps permite ejecutar microservicios y aplicaciones contenedorizadas en una plataforma sin servidor. Con Container Apps, puede disfrutar de las ventajas de ejecutar contenedores y, al mismo tiempo, dejar atrás las preocupaciones de configurar manualmente la infraestructura en la nube y orquestadores de contenedores complejos.

## Objetivos

En este laboratorio, aprenderemos a:
- Tarea 1: Creación de una aplicación de contenedor y un entorno
- Tarea 2: Implementación de la aplicación contenedorizada
- Tarea 3: Prueba y verificación de la implementación de la aplicación contenedorizada

Para empezar, inicie sesión en [Azure Portal](https://portal.azure.com).

## Tiempo estimado: 20 minutos

## Tarea 1: Creación de una aplicación de contenedor y un entorno

Para crear una aplicación contenedora, comience en la página principal de Azure Portal.

1. Busque `Container Apps` en la barra de búsqueda superior.
1. Busque **Aplicaciones de contenedor** en los resultados de la búsqueda.
1. Seleccione el botón **Crear**.

### Pestaña Aspectos básicos

En la pestaña *Aspectos básicos*, realice las acciones siguientes.

1. Escriba los siguientes valores en la sección *Detalles del proyecto*.

    | Configuración | Acción |
    |---|---|
    | Subscription | Seleccione su suscripción a Azure. |
    | Resource group | Seleccione **Crear nuevo** y escriba `az104-09c-rg1`. |
    | Nombre de la aplicación de contenedor |  Escriba `my-container-app`. |

#### Creación de un entorno

A continuación, cree un entorno para la aplicación de contenedor.

1. Seleccione la región apropiada.

    | Configuración | Value |
    |--|--|
    | Region | **Su elección**. |

1. En el campo *Create Container App environment* (Crear entorno de aplicaciones de contenedor), seleccione el vínculo **Create new** (Crear nuevo).
1. En la página *Create Container App environment* (Crear entorno de aplicaciones de contenedor), en la pestaña *Basics* (Aspectos básicos), escriba los siguientes valores:

    | Configuración | Value |
    |--|--|
    | Nombre del entorno | Escriba `my-environment`. |
    | Redundancia de zona | Seleccione **Deshabilitado** |

1. Seleccione la pestaña **Supervisión** para crear un área de trabajo de Log Analytics.
1. Seleccione el vínculo **Crear nuevo** en el campo *Área de trabajo de Log Analytics* y escriba los siguientes valores.

    | Configuración | Value |
    |--|--|
    | Nombre | Escriba `my-container-apps-logs`. |
  
    El campo *Ubicación* se ha rellenado previamente con su región.

1. Seleccione **Aceptar**.


## Tarea 2: Implementación de la aplicación contenedorizada

1. Seleccione el botón **Revisar y crear** de la parte inferior de la página.  

    A continuación, se comprueba la configuración de la aplicación de contenedor. Si no se encuentran errores, se habilita el botón *Crear*.  

    Si hay errores, todas las pestañas que contengan errores se marcan con un punto rojo.  Vaya a la pestaña adecuada. Los campos que contienen un error se resaltarán en rojo.  Una vez corregidos todos los errores, vuelva a seleccionar **Revisar y crear**.

1. Seleccione **Crear**.

    Se muestra una página con el mensaje *Implementación en curso*.  Una vez completada correctamente la implementación, verá el mensaje: *Se completó la implementación*.
   
## Tarea 3: Prueba y verificación de la implementación de la aplicación contenedorizada

Seleccione **Ir al recurso** para ver la aplicación de contenedor.  Seleccione el vínculo que hay junto a *URL de aplicación* para ver la aplicación. Compruebe que aparece el mensaje *Bienvenido a Azure Container Apps*.

## Limpieza de recursos

Si no va a seguir usando esta aplicación, puede eliminar la instancia de Azure Container Apps y todos los servicios asociados quitando el grupo de recursos.

1. Seleccione el grupo de recursos **my-container-apps** en la sección *Información general*.
1. Seleccione el botón **Eliminar grupo de recursos** en la parte superior de la página del grupo de recursos *Información general*.
1. Escriba el nombre del grupo de recursos y confirme que desea eliminar la aplicación. 
1. Seleccione **Eliminar**.
1. El proceso para eliminar el grupo de recursos puede tardar unos minutos en completarse.
