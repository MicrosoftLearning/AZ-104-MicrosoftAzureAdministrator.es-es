---
lab:
  title: "Laboratorio\_07: Administración de Azure\_Storage"
  module: Administer Azure Storage
---

# Laboratorio 07: Administración de Azure Storage

## Introducción al laboratorio

En este laboratorio aprenderá a crear cuentas de almacenamiento para blobs y archivos de Azure. Aprenderá a configurar y proteger contenedores de blobs. También aprenderá a usar el explorador de almacenamiento para configurar y proteger recursos compartidos de archivos de Azure. 

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.**

## Tiempo estimado: 50 minutos

## Escenario del laboratorio

La organización almacena actualmente datos en almacenes de datos locales. A la mayoría de estos archivos no se accede con frecuencia. Para minimizar el coste de almacenamiento, quisiera colocar los archivos a los que se acceda con poca frecuencia en niveles de almacenamiento de menor precio. También tiene previsto explorar los diferentes mecanismos de protección que ofrece Azure Storage, incluido el acceso a la red, la autenticación, la autorización y la replicación. Por último, quisiera determinar en qué medida el servicio Azure Files sería adecuado para hospedar los recursos compartidos de archivos locales.

## Simulaciones interactivas de laboratorio

Hay simulaciones de laboratorio interactivas que podrían resultar útiles para este tema. La simulación le permite hacer clic en un escenario similar a su propio ritmo. Hay ciertas diferencias entre la simulación interactiva y este laboratorio, pero muchos de los conceptos básicos son los mismos. No se necesita una suscripción de Azure. 

+ [Creación del almacenamiento de blobs](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205). Cree una cuenta de almacenamiento, administre el almacenamiento de blogs y supervise las actividades de almacenamiento. 
  
+ [Administración del almacenamiento de Azure](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011). Cree una cuenta de almacenamiento y revise la configuración. Administrar contenedores de Blob Storage. Configuración de conexiones en red de almacenamiento. 

## Diagrama de la arquitectura

![Diagrama de las tareas.](../media/az104-lab07-architecture.png)

## Aptitudes de trabajo

+ Tarea 1: Crear y configurar una cuenta de almacenamiento. 
+ Tarea 2: Creación y configuración del almacenamiento de blobs seguro.
+ Tarea 3: Creación y configuración del almacenamiento seguro de archivos de Azure.

## Tarea 1: Crear y configurar una cuenta de almacenamiento. 

En esta tarea, creará y configurará una cuenta de almacenamiento. La cuenta de almacenamiento usará almacenamiento con redundancia geográfica y no tendrá acceso público. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `Storage accounts` y, a continuación, haga clic en **+ Crear**.

1. En la pestaña **Aspectos básicos** del panel **Crear cuenta de almacenamiento**, configure las siguientes opciones (deje las demás con los valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Suscripción          | el nombre de la suscripción de Azure  |
    | Resource group        | **az104-rg7** (crear nueva) |
    | Nombre de la cuenta de almacenamiento  | Cualquier nombre globalmente único con una longitud de 3 a 24 caracteres, que consta de letras y dígitos |
    | Región                | **(EE. UU.) Este de EE. UU.**  |
    | Rendimiento           | **Estándar** (observe la opción Premium) |
    | Redundancia            | **Almacenamiento con redundancia geográfica** (observe las otras opciones)|
    | Habilitar el acceso de lectura a los datos en caso de disponibilidad regional | Active la casilla. |

>**¿Sabía que...?** Se debería usar el nivel de rendimiento Estándar para la mayoría de las aplicaciones. Use el nivel de rendimiento Premium para aplicaciones empresariales o de alto rendimiento. 

1. En la pestaña **Opciones avanzadas**, use los iconos informativos para obtener más información sobre las opciones. Use los valores predeterminados. 

1. En la pestaña **Redes**, revise las opciones disponibles y seleccione **Deshabilitar el acceso público y usar el acceso privado**.

1. Revise la pestaña **Protección de datos**. Observe que 7 días es la directiva de retención de eliminación temporal predeterminada. Tenga en cuenta que es posible habilitar el control de versiones de blobs. Acepte los valores predeterminados.

1. Revise la pestaña **Cifrado**. Observe las opciones de seguridad adicionales. Acepte los valores predeterminados.

1. Seleccione **Revisar**, espere a que se complete el proceso de validación y, a continuación, haga clic en **Crear**.

1. Una vez implementada la cuenta de almacenamiento, seleccione **Ir al recurso**.

1. Revise la hoja **Información general** y las configuraciones adicionales que se pueden cambiar. Se trata de una configuración global para la cuenta de almacenamiento. Observe que la cuenta de almacenamiento se puede usar para contenedores de blobs, recursos compartidos de archivos, colas y tablas.

1. En la sección **Seguridad y redes**, seleccione **Redes**. Observe que el acceso a la red pública está deshabilitado.

    + Cambie el **nivel de acceso público** a **Habilitado desde redes virtuales y direcciones IP seleccionadas**.
    + En la sección **Firewall**, active la casilla **Agregar la dirección IP del cliente.**
    + Asegúrese de **Guardar** los cambios. 
  
1. En la sección **Administración de datos**, observe la hoja **Redundancia**. Observe la información sobre las ubicaciones del centro de datos principal y secundario.

1. En la sección **Administración de datos**, seleccione **Administración del ciclo de vida** y, a continuación, seleccione **Agregar una regla**.

    + **Asigne un nombre** a la regla `Movetocool`. Observe las opciones para limitar el ámbito de la regla.
    
    + En la pestaña **Blobs de base**, *si* los blobs de base se modificaron por última vez más de hace más de `30 days`, *entonces* **mueva a almacenamiento esporádico**. Observe las otras opciones. 
    
    + Observe que se pueden configurar otras condiciones. Seleccione **Agregar** cuando haya terminado de explorar.

    ![Captura de pantalla sobre mover a condiciones de regla esporádicos.](../media/az104-lab07-movetocool.png)

## Tarea 2: Creación y configuración del almacenamiento de blobs seguro

En esta tarea, creará un contenedor de blobs y cargará una imagen. Los contenedores de blobs son estructuras de tipo directorio que almacenan datos no estructurados.

### Creación de contenedores de blobs y directivas de retención basadas en el tiempo

1. Continúe en Azure Portal, trabajando con la cuenta de almacenamiento.

1. En la sección **Almacenamiento de datos**, haga clic en **Contenedores**. 

1. Haga clic en **+ Contenedor** y **Cree** un contenedor con la configuración siguiente:

    | Configuración | Value |
    | --- | --- |
    | Nombre | `data`  |
    | Nivel de acceso público | Observe que el nivel de acceso esté establecido en privado |

    ![Captura de pantalla de la creación de un contenedor.](../media/az104-lab07-create-container.png)

1. En el contenedor, desplácese hasta los puntos suspensivos (...) del lado extremo derecho y seleccione **Directiva de acceso**.

1. En la sección **Almacenamiento de blobs inmutable**, seleccione **Agregar directiva**.

    | Configuración | Valor |
    | --- | --- |
    | Tipo de directiva | **Retención con duración definida**  |
    | Establecer el período de retención para | `180` días |

1. Seleccione **Guardar**.

### Administración de cargas de blobs

1. Vuelva a la página contenedores, seleccione el contenedor de **datos** y haga clic en **Cargar**.

1. En la hoja **Actualizar blob**, expanda la sección **Avanzado**.

    >**Nota**: Busque el archivo que se vaya a cargar. Puede ser cualquier tipo de archivo, pero un archivo pequeño resultará mejor. Se puede descargar un archivo de ejemplo del directorio AllFiles. 

    | Configuración | Valor |
    | --- | --- |
    | Buscar archivos | agregue el archivo que haya seleccionado para cargar |
    | Seleccione **Avanzado**. | |
    | Tipo de blob | **Blob en bloques** |
    | Tamaño de bloque | **4 MiB** |
    | Nivel de acceso | **Frecuente**  (observe las otras opciones) |
    | Cargar en carpeta | `securitytest` |
    | Ámbito de cifrado | Usar el ámbito del contenedor predeterminado existente |

1. Haga clic en **Cargar**.

1. Confirme que tenga una nueva carpeta y que el archivo se cargó. 

1. Seleccione el archivo de carga y revise las opciones, como **Descargar**, **Eliminar**, **Cambiar de nivel** y **Adquirir concesión**.

1. Copie la **dirección URL** del archivo y péguela en una nueva ventana de exploración de **Inprivate**.

1. Debería presentarse un mensaje con formato XML que indique **ResourceNotFound** o **PublicAccessNotPermitted**.

    > **Nota**: Esto es lo esperado, ya que el contenedor que creó tiene el nivel de acceso público establecido en **Privado (sin acceso anónimo)** .

### Configuración del acceso limitado al almacenamiento de blobs

1. Seleccione el archivo cargado y la pestaña **Generar SAS**. También es posible usar los puntos suspensivos (...) del lado extremo derecho. Especifique las opciones de configuración siguientes (deje las demás con los valores predeterminados):

    | Configuración | Value |
    | --- | --- |
    | Clave de firma | **Clave 1** |
    | Permisos | **Leer** (observe las otras opciones) |
    | Fecha de inicio | fecha de ayer |
    | Hora de inicio | hora actual |
    | Fecha de vencimiento | fecha de mañana |
    | Hora de expiración | hora actual |
    | Direcciones IP permitidas | déjelo en blanco |

1. Haga clic en **Generar URL y token de SAS**.

1. Copie la entrada **dirección URL de SAS de blob** en el Portapapeles.

1. Abra otra ventana del explorador en InPrivate y vaya a la dirección URL de SAS de blob que copió en el paso anterior.

    >**Nota**: Debería poder ver el contenido del archivo. 

## Tarea 3: Creación y configuración del almacenamiento de archivos de Azure

En esta tarea, creará y configurará recursos compartidos de Azure. Usará el explorador de almacenamiento para administrar el recurso compartido. 

### Creación del recurso compartido y carga de un archivo

1. En Azure Portal, vuelva a la cuenta de almacenamiento, en la sección **Almacenamiento de datos** y haga clic en **Recursos compartidos**.

1. Haga clic en **+ Recurso compartido** y, en la pestaña **Aspectos básicos**, asigne un nombre al recurso compartido de archivos, `share1`. 

1. Observe las opciones de **Nivel de acceso**. Mantenga la opción predeterminada **Optimizado para transacciones**.
   
1. Vaya a la pestaña **Copia de seguridad** y asegúrese de que la opción **Habilitar copia de seguridad** **no** esté activada. Estamos deshabilitando la copia de seguridad para simplificar la configuración del laboratorio.

1. Haga clic en **Revisar y crear** y, a continuación, en **Crear**. Espere a que el recurso compartido de archivos se implemente.

    ![Captura de pantalla de la página Crear recurso compartido.](../media/az104-lab07-create-share.png)

### Exploración del explorador de almacenamiento y carga de un archivo

1. Vuelva a la cuenta de almacenamiento y seleccione **Explorador de almacenamiento**. El explorador de almacenamiento de Azure es una herramienta del portal que permite ver rápidamente todos los servicios de almacenamiento de la cuenta.

1. Seleccione **Recursos compartidos** y compruebe que el directorio **share1** esté presente.

1. Seleccione el directorio **share1** y observe que pueda **+ Agregar directorio**. Esto le permitirá crear una estructura de carpetas.

1. Seleccione **Cargar**. Vaya a un archivo de su elección y, a continuación, haga clic en **Cargar**.

    >**Nota**: Es posible ver los recursos compartidos y administrarlos en el explorador de almacenamiento. En este momento no hay restricciones.

### Restricción del acceso de red a la cuenta de almacenamiento

1. En el portal, busque y seleccione **Redes virtuales**.

1. Seleccione **+ Create** (+ Crear). Seleccione el grupo de recursos. y asigne a la red virtual un **nombre**, `vnet1`.

1. Tome los valores predeterminados para otros parámetros, seleccione **Revisar y crear** y, a continuación, **Crear**.

1. Espere a que se implemente la red virtual y seleccione **Ir al recurso**.

1. En la sección **Configuración**, seleccione la hoja **Puntos de conexión de servicio**.
    + Seleccione **Agregar**. 
    + En la lista desplegable **Servicios**, seleccione **Microsoft.Storage**.
    + En la lista desplegable **Subredes**, active la subred **Predeterminada**.
    + Haga clic en **Agregar** para guardar los cambios.  

1. Vuelva a la cuenta de almacenamiento.

1. En la sección **Seguridad y redes**, seleccione la hoja **Redes**.

1. Seleccione **Agregar red virtual existente**, así como **vnet1** y subred **predeterminada**. Seleccione **Agregar**.

1. En la sección **Firewall**, **Elimine** la dirección IP de la máquina. El tráfico permitido solo debería provenir de la red virtual. 

1. Asegúrese de **Guardar** los cambios.

    >**Nota:** Ahora solo se debería acceder a la cuenta de almacenamiento desde la red virtual que acaba de crear. 

1. Seleccione el **Explorador de almacenamiento** y **Actualice** la página. Vaya al recurso compartido o al contenido del blob.  

    >**Nota:** Debería recibir un mensaje *No autorizado para realizar esta operación*. No se está conectando desde la red virtual. Esto podría tardar un par de minutos en surtir efecto.


![Captura de pantalla del acceso no autorizado.](../media/az104-lab07-notauthorized.png)

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Ampliar el aprendizaje con Copilot
Copilot puede ayudarle a aprender a usar las herramientas de scripting de Azure. Copilot también puede ayudar en áreas no cubiertas en el laboratorio o donde necesita más información. Abra un explorador Edge y elija Copilot (superior derecha) o vaya a *copilot.microsoft.com*. Dedique unos minutos a probar estas indicaciones.

+ Proporcione un script de Azure PowerShell para crear una cuenta de almacenamiento con un contenedor de blobs. 
+ Proporcione una lista de comprobación que pueda usar para asegurarse de que mi cuenta de almacenamiento de Azure sea segura.
+ Cree una tabla para comparar los modelos de redundancia de Azure Storage.

## Más información con el aprendizaje autodirigido

+ [Optimice los costes con Azure Blob Storage.](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/). Aprenda a optimizar los costes con Azure Blob Storage.
+ [Control de acceso a Azure Storage con firmas de acceso compartido](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/). Conceda acceso de forma segura a los datos almacenados en las cuentas de Azure Storage mediante firmas de acceso compartido.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ Una cuenta de almacenamiento de Azure contiene todos los objetos de datos de Azure Storage: blobs, archivos, colas y tablas. La cuenta de almacenamiento proporciona un espacio de nombres único para los datos de Azure Storage que es accesible desde cualquier lugar del mundo a través de HTTP o HTTPS.
+ El almacenamiento de Azure proporciona varios modelos de redundancia, incluyendo el almacenamiento con redundancia local (LRS), el almacenamiento con redundancia de zona (ZRS) y el almacenamiento con redundancia geográfica (GRS). 
+ Azure Blob Storage permite almacenar grandes cantidades de datos no estructurados en la plataforma de almacenamiento de datos de Microsoft. Blob significa "Binary Large Object" (objeto binario grande), lo cual incluye objetos como imágenes y archivos multimedia.
+ Azure File Storage proporciona almacenamiento compartido para datos estructurados. Los datos se pueden organizar en carpetas.
+ El almacenamiento inmutable permite almacenar datos con el estado WORM (una escritura, múltiples lecturas). Las directivas de almacenamiento inmutable pueden basarse en tiempo o en retención legal.
