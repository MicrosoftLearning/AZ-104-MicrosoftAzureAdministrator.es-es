---
demo:
  title: "Demostración\_07: Administración de Azure Storage"
  module: Administer Azure Storage
---


# 07: Administración de Azure Storage

## Configuración de cuentas de almacenamiento

En esta demostración, crearemos una cuenta de almacenamiento.

**Referencia**: [Creación de una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Use Azure Portal.

1. Revise el propósito de las cuentas de almacenamiento. 
   
1. Busque y seleccione **Cuentas de almacenamiento**. 
 
1. Cree una cuenta de almacenamiento básica. 

    - Analice los requisitos para asignar un nombre a una cuenta de almacenamiento y la necesidad de que el nombre sea único en Azure. 

    - Revise los distintos tipos de almacenamiento. Por ejemplo, de uso general v2. 

    - Revise las selecciones de nivel de acceso. Por ejemplo, los niveles de acceso esporádico y frecuente. 

    - Las demás pestañas y configuraciones se tratarán en otras demostraciones. 

1. Cree la cuenta de almacenamiento y espere a que se implemente el recurso. 


## Configuración de Azure Blob Storage

En esta demostración, se explorará el almacenamiento de blobs.

**Nota:**  Para estos pasos se necesita una cuenta de almacenamiento.

**Referencia**: [Inicio rápido: Carga, descarga y enumeración de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Navegue hasta una cuenta de almacenamiento en Azure Portal.

1. Revise el propósito del almacenamiento de blobs. 

1. Cree un contenedor de blobs. Revise el nivel de acceso del contenedor. Por ejemplo, privado (sin acceso anónimo). 

1. Cargue un blob en el contenedor. Como tiene tiempo, revise la configuración avanzada. Por ejemplo, el tipo de blob y el tamaño del blob. 

## Configuración de la seguridad de almacenamiento

En esta demostración, se creará una firma de acceso compartido.

**Nota:**  Para esta demostración se necesita una cuenta de almacenamiento, con un contenedor de blobs y un archivo cargado.

**Referencia**: [Creación de tokens de SAS para contenedores de almacenamiento](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Seleccione un blob o archivo que quiera proteger. 

1. Generar una firma de acceso compartido (SAS). Revise los permisos, los tiempos de inicio y expiración y los protocolos permitidos.

1. Use la dirección URL de SAS para asegurarse de que se muestra el recurso. 


## Configuración de Azure Files 

En esta demostración, se trabajará con instantáneas y recursos compartidos de archivos.

**Nota:**  Para estos pasos se necesita una cuenta de almacenamiento.

**Referencia**: [Inicio rápido para administrar recursos compartidos de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Revise el propósito de los recursos compartidos de archivos. 

1. Acceda a una cuenta de almacenamiento y haga clic en  **Archivos**.

1. Creación de un recurso compartido de archivos. Revise las cuotas, cargue archivos y agregue directorios para organizar la información. 

1. Cree una instantánea de recurso compartido de archivos. Revise cuándo usar instantáneas y en qué se diferencian de las copias de seguridad. Como tiene tiempo, cargue un archivo, tome una instantánea, elimine el archivo y restaure la instantánea.


## Herramientas de almacenamiento (opcional)

En esta demostración, se revisarán varias herramientas de almacenamiento comunes de Azure. 

**Referencia**: [Introducción al Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Instale el Explorador de Storage o úselo.

1. Revise cómo examinar y crear recursos de almacenamiento. Por ejemplo, agregue un contenedor de blobs. 

**Referencia**: [Copia o transferencia de datos a Azure Storage con AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Analice cuándo usar AzCopy. Vea la ayuda, **azcopy /?** .

1. Desplácese hacia abajo en la sección  **Ejemplos**. Como tiene tiempo, pruebe cualquiera de los ejemplos. 
    



