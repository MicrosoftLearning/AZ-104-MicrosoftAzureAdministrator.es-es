---
demo:
  title: "Demostración\_10: Administración de la protección de datos"
  module: Administer Data Protection
---

# 10: Administración de la protección de datos

## Copia de seguridad de los recursos compartidos de archivos de Azure

En esta demostración, se explorará la copia de seguridad de un recurso compartido de archivos en Azure Portal.

> **Nota:** Para esta demostración se necesita una cuenta de almacenamiento con un recurso compartido de archivos. 

**Referencia**: [Copia de seguridad de los recursos compartidos de archivos de Azure en Azure Portal](https://docs.microsoft.com/azure/backup/backup-afs)

**Creación de un almacén de Recovery Services**

1. Use Azure Portal.

1. Busque **Almacenes de Recovery Services** y selecciónelo.

1. Cree un **Almacén de Recovery Services**. Revise el requisito de que el almacén esté en la misma región que el recurso compartido de archivos. 

1. Espere a que se cree el almacén. 

**Configuración de la copia de seguridad de archivos de Azure**

1. Vaya al **Centro de copias de seguridad** y cree una nueva instancia de **Backup**.

1. Revise y analice las opciones en la lista desplegable **Tipo de origen de datos**. Seleccione **Azure Files (Azure Storage)** . 

1. Seleccione el **almacén**.

1. **Continúe** configurando la copia de seguridad. Seleccione la cuenta de almacenamiento específica y el recurso compartido de archivos del que desea realizar una copia de seguridad.  

1. En **Detalles de la directiva**, haga clic en **Editar esta directiva**. Analice el propósito de las directivas de copia de seguridad. Revise la **Programación de copia de seguridad** y la **Duración de retención**.  

1. **Habilite la copia de seguridad** para guardar los cambios. 

1. Como tiene tiempo, revise cómo **restaurar** una**instancia de Backup**. Lea también cómo supervisar los **trabajos de Backup**. 

## Copia de seguridad de Azure Virtual Machines

En esta demostración, se programará una copia de seguridad diaria de una máquina virtual en un almacén de Recovery Services.

> **Nota:** En esta demostración se necesita una máquina virtual y un almacén de Recovery Services.

**Referencia**: [Tutorial: Copia de seguridad de varias máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/tutorial-backup-vm-at-scale)

1. Use Azure Portal.

1. Vaya al **Centro de copias de seguridad** y cree una nueva instancia de **Backup**.

1. Seleccione **Azure Virtual Machines** en **Tipo de origen de datos** y, después, seleccione el almacén que ha creado.

1. Revise la **Directiva predeterminada**. La directiva predeterminada hace una copia de seguridad de la máquina virtual una vez al día. Asimismo, las copias de seguridad diarias se conservan durante 30 días. Las instantáneas de recuperación instantánea se conservan durante dos días.

1. Use **Habilitar copia de seguridad** para guardar la configuración.

1. Como tiene tiempo, revise cómo **realizar una copia de seguridad ahora**. Lea también cómo revisar los **trabajos de Backup**.  

