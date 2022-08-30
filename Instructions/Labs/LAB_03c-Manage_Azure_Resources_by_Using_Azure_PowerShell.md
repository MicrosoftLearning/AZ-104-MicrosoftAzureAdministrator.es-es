---
lab:
  title: '03c: Administración de recursos de Azure mediante Azure PowerShell'
  module: Module 03 - Azure Administration
---

# <a name="lab-03c---manage-azure-resources-by-using-azure-powershell"></a>Laboratorio 03c: Administración de recursos de Azure mediante Azure PowerShell
# <a name="student-lab-manual"></a>Manual de laboratorio para alumnos

## <a name="lab-scenario"></a>Escenario del laboratorio

Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal and Azure Resource Manager templates, you need to carry out the equivalent task by using Azure PowerShell. To avoid installing Azure PowerShell modules, you will leverage PowerShell environment available in Azure Cloud Shell.

## <a name="objectives"></a>Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Iniciar una sesión de PowerShell en Azure Cloud Shell
+ Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante Azure PowerShell
+ Tarea 3: Configurar el disco administrado mediante Azure PowerShell

## <a name="estimated-timing-20-minutes"></a>Tiempo estimado: 20 minutos

## <a name="architecture-diagram"></a>Diagrama de la arquitectura

![imagen](../media/lab03c.png)

## <a name="instructions"></a>Instrucciones

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Always create your own secure password for any virtual machine or user account you create. If the virtual machine is created for you, use <bpt id="p1">**</bpt>Reset password<ept id="p1">**</ept> in the Portal to update the password. 

### <a name="exercise-1"></a>Ejercicio 1

#### <a name="task-1-start-a-powershell-session-in-azure-cloud-shell"></a>Tarea 1: Iniciar una sesión de PowerShell en Azure Cloud Shell

En esta tarea, abrirá una sesión de PowerShell en Cloud Shell. 

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**. 

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**. 

1. Si se le solicite, haga clic en **Crear almacenamiento** y espere hasta que aparezca Azure Cloud Shell. 

1. Asegúrese de que **PowerShell** aparezca en el menú desplegable superior izquierdo del panel de Cloud Shell.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-powershell"></a>Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante Azure PowerShell

En esta tarea, creará un grupo de recursos y un disco administrado de Azure mediante una sesión de Azure PowerShell dentro de Cloud Shell.

1. Para crear un grupo de recursos en la misma región de Azure que el grupo de recursos **az104-03b-rg1** que creó en el laboratorio anterior, desde la sesión de PowerShell en Cloud Shell, ejecute lo siguiente:

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Para recuperar las propiedades del grupo de recursos recién creado, ejecute lo siguiente:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Para crear un nuevo disco administrado con las mismas características que las que creó en los laboratorios anteriores de este módulo, ejecute lo siguiente:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Para recuperar las propiedades del disco recién creado, ejecute lo siguiente:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

#### <a name="task-3-configure-the-managed-disk-by-using-azure-powershell"></a>Tarea 3: Configurar el disco administrado mediante Azure PowerShell

En esta tarea, administrará la configuración del disco administrado de Azure mediante una sesión de Azure PowerShell en Cloud Shell. 

1. Para aumentar el tamaño del disco administrado de Azure a **64 GB**, desde la sesión de PowerShell en Cloud Shell, ejecute lo siguiente:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Para comprobar la SKU actual como **Standard_LRS**, ejecute lo siguiente:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Para cambiar la SKU de rendimiento del disco a **Premium_LRS**, desde la sesión de PowerShell en Cloud Shell, ejecute lo siguiente:

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

#### <a name="clean-up-resources"></a>Limpieza de recursos

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>Revisar

En este laboratorio, ha:

- Iniciado una sesión de PowerShell en Azure Cloud Shell
- Creado un grupo de recursos y un disco administrado de Azure mediante Azure PowerShell
- Configurado el disco administrado mediante Azure PowerShell
