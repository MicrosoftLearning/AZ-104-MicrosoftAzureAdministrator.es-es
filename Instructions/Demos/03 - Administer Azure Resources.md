---
demo:
  title: "Demostración\_03: Administración de recursos de Azure"
  module: Administer Administer Azure Resources
---
# 03: Administración de recursos de Azure

## Demostración: Azure Portal

En esta demostración, se explorará Azure Portal.

**Referencia**: [Administración de las preferencias y la configuración de Azure Portal](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**Referencia**: [Creación de un panel en Azure Portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**Referencia**: [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. Acceda a Azure Portal.

1. Seleccione el icono  **Soporte técnico y solución de problemas** en el banner superior. Revise los vínculos **Recursos de soporte técnico**. 

1. Seleccione el icono **Configuración** en el banner superior.  Revise la configuración de **Apariencia y vistas de inicio**. 

1. Use el menú de la izquierda y seleccione **Panel**. **Edite** el panel mediante la **Galería de iconos**. Analice las opciones de personalización. 

1. Pregunte si los alumnos tienen alguna pregunta sobre cómo usar Azure Portal. 

## Demostración: Cloud Shell

En esta demostración, se experimentará con Cloud Shell.

**Referencia**: [Inicio rápido para Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**Configuración de Cloud Shell**

1.  Acceda a  **Azure Portal**.

1.  Haga clic en el icono de  **Cloud Shell** en el banner superior.

1.  En la página Bienvenido a Azure Cloud Shell, observe las selecciones para Bash o PowerShell.  Seleccione  **PowerShell**.

1.  Analice por qué Azure Cloud Shell requiere un recurso compartido de archivos de Azure para conservar los archivos. Si es necesario, configure el recurso compartido de almacenamiento. 

**Experimentación con Azure PowerShell y Bash**

1. Asegúrese de que el shell de **PowerShell** está seleccionado y pruebe algunos comandos. Por ejemplo, **Get-AzSubscription** y **Get-AzResourceGroup**.

1. Muestra cómo funciona la característica de autocompletar. Muestra cómo borrar la pantalla, **cls**. 

1. Asegúrese de que el shell de **Bash** está seleccionado y pruebe algunos comandos. Por ejemplo, **az account list** y **az resource list**.

1. Pregunte si los alumnos tienen alguna pregunta sobre el uso de los comandos de PowerShell o Bash. 

**Experimentación con el editor de Cloud Shell (opcional)**

1. Para usar el editor de Cloud, seleccione el icono de las **llaves**.

1. Seleccione un archivo en el panel de navegación de la izquierda.  Por ejemplo,  **.profile**.

1. Observe en el banner superior del editor las selecciones para Configuración (Tamaño de texto y Fuente) y Carga/descarga de archivos.

1. Observe los puntos suspensivos ( **\...** ) en el extremo derecho para **Guardar**, **Cerrar editor** y **Abrir archivo**.

1. Experimente a medida que tenga tiempo y, después,  **cierre** el editor de Cloud.

1. Cierre Cloud Shell.

## Demostración: Plantillas de inicio rápido

En esta demostración, se explorarán las plantillas de inicio rápido.

**Referencia**: [Tutorial: Creación e implementación de una plantilla (Azure Resource Manager)](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. Para empezar, vaya a la  [galería de plantillas de inicio rápido de Azure](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager). Observe que hay ejemplos de JSON y Bicep. 

1. Pregunte a los alumnos si hay plantillas específicas que sean de su interés. Si no hay ninguna, seleccione una plantilla. Por ejemplo, la plantilla [Implementación de una máquina virtual Windows sencilla con etiquetas](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/).

1. Analice cómo el botón  **Implementar en Azure** permite implementar la plantilla directamente desde Azure Portal.

1. **Implemente** la plantilla JSON y analice cómo puede editar el archivo de plantillas y parámetros. Revise el propósito de los archivos. Como tiene tiempo, revise la sintaxis. 

1. Vuelva a la galería de ejemplos de código y busque una plantilla de Bicep. Por ejemplo, [Creación de una cuenta de Standard Storage](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/). 

1. **Implemente** la plantilla Bicep y analice cómo puede editar el archivo de plantillas y parámetros. Como tiene tiempo, revise la sintaxis. 
