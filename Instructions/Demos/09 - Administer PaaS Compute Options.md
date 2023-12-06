---
demo:
  title: "Demostración\_09: Administración de opciones de proceso de PaaS"
  module: Administer PaaS Compute Options
---

# 09: Administración de opciones de proceso de PaaS

## Configuración de planes de Azure App Service

En esta demostración, creará y trabajará con planes de Azure App Service.

**Referencia**: [Administración de un plan de App Service: Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**Referencia**: [Escalado de una aplicación en Azure App Service](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**Referencia**: [Escalado automático en Azure App Service](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. Use Azure Portal. 

1. Busque y seleccione  **Planes de App Service**.

1. Cree un plan de App Service sencillo. Analice la necesidad de seleccionar Windows o Linux. Analice los planes de precios ahora o en los pasos siguientes. 

1. Implemente el nuevo plan de App Service. 

1. Revise el panel **Escalar verticalmente (plan de App Service)** . Analice la diferencia entre los planes **Desarrollo/pruebas** y **Producción**. Revise la lista de características. 

1. Revise el panel **Escalar horizontalmente (plan de App Service)** . Revise la diferencia entre **Manual** y **Basado en reglas**. 

## Configuración de Azure App Services

En esta demostración, se creará una aplicación web que ejecuta un contenedor de Docker. El contenedor muestra un mensaje de bienvenida.

**Referencia**: [Creación de una aplicación web](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

En esta tarea, crearemos una aplicación web de Azure App Service.

1. Use Azure Portal. 

1. Busque y seleccione **App Services**.

1. **Cree** una **aplicación web**.

    - Publique: **Código**. Revise otras opciones.
    - Pila en tiempo de ejecución: **.Net**. Revise otras opciones.
    - Sistema operativo: **Linux**

1. Seleccione el plan de servicio **F1 Gratis**.

1. **Revise y cree** la aplicación web. Espere a que se implemente el recurso.

1. En la página **Información general**, asegúrese de que el **Estado** es **En ejecución**.

1. Seleccione la **Dirección URL** y asegúrese de que se carga la página de marcador de posición predeterminada.

1. Como tiene tiempo, explore las opciones de **Ranuras de implementación**.
   
## Configuración de Azure Container Instances

En esta demostración se creará, configurará e implementará un contenedor mediante Azure Container Instances (ACI) en Azure Portal. La aplicación ACI muestra una página HTML estática con la imagen pública de Microsoft Hola mundo. 

**Referencia**: [Inicio rápido: implementación de un contenedor de Docker en una instancia de contenedor](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. Use Azure Portal.

1. Busque y seleccione  **Instancias de contenedor**.

1. **Cree** una instancia de contenedor. 

1. Rellene el **Grupo de recursos** y el **Nombre del contenedor**. 

1. Analice las opciones de **Origen de la imagen**. Utilice **Imágenes de inicio rápido**.

1. Para **Imagen de contenedor**, use **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** . Esta imagen de Linux de ejemplo empaqueta una pequeña aplicación web escrita en Node.js que sirve una página HTML estática.

1. En la página **Redes**, especifique una **etiqueta de nombre DNS** para el contenedor. 

1. Deje el resto de opciones con sus valores predeterminados y, luego, seleccione **Revisar y crear**.

1. Espere a que se implemente el recurso.

1. En la página **Información general** del recurso, asegúrese de que el **Estado** es **En ejecución**.

1. Vaya al **FQDN** de la instancia de contenedor y asegúrese de que se muestra la página principal. 

**Nota**: Para evitar costos adicionales, elimine el recurso. 

## Configuración de Azure Container Apps

En esta demostración, creará y trabajará con Azure Container Apps. 

**Referencia**: [Inicio rápido: Implementación de la primera aplicación de contenedor mediante Azure Portal](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. Busque y seleccione **Container Apps**.

1. Complete los **detalles del proyecto** y cree el **entorno** de las aplicaciones de contenedor.

1. **Revise y cree** la aplicación de contenedor.

1. Use el vínculo **Dirección URL de la aplicación** para ver la aplicación.

1. Compruebe que el explorador muestra el mensaje **Bienvenido a Azure Container Apps**. 






