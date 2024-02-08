---
demo:
  title: "Demostración\_08: Administración de Azure Virtual Machines"
  module: Administer Azure Virtual Machines
---


# 08: Administración de Azure Virtual Machines

## Demostración: Creación de máquinas virtuales en el portal

En esta demostración, se creará y accederá a una máquina virtual de Azure en el portal.

**Referencias**

[Guía de inicio rápido: creación de una máquina virtual Windows en Azure Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Guía de inicio rápido: creación de una máquina virtual Linux en el Azure Portal](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Conexión a una máquina virtual con Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Creación de la máquina virtual**

**Nota:** En estos pasos solo se describen algunos parámetros de máquina virtual. No dude en explorar otras áreas. Puede crear una máquina virtual Windows o Linux, en función de su audiencia.

1. Use Azure Portal.

1. Busque **Máquinas virtuales**. 

1. Cree una máquina virtual básica. Revise las opciones de disponibilidad, las imágenes y las reglas de entrada.

1. Analice la importancia de crear una cuenta de administrador segura.

1. Cree la máquina virtual y espere a que se implemente el recurso.  

**Conexión a la máquina virtual**

1. Hay varias maneras de **conectarse** a la máquina virtual. 

1. Para un servidor Windows, puede usar **RDP**, como se muestra en el inicio rápido. 

1. Para un servidor Linux, puede usar **SSH**, como se muestra en el inicio rápido. 

1. Para cualquiera de los servidores, puede conectarse con el servicio **Bastion** (inicio rápido). Revise por qué se prefiere Bastion a RDP o SSH. 

## Configuración de la disponibilidad de las máquinas virtuales

En esta demostración, se explorarán las opciones de escalado de máquina virtual.

**Referencias**

[Creación de máquinas virtuales en un conjunto de escalado mediante Azure Portal](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Use Azure Portal.

1. Busque y seleccione **Conjuntos de escalado de máquinas virtuales**. 

1. Cree un **Conjunto de escalado de máquinas virtuales**. Revise el propósito de los conjuntos de escalado de máquinas virtuales. Revise la diferencia entre los modos de orquestación **Uniforme** y **Flexible**. Explique que la selección puede afectar a las opciones de escalado. 

1. Vaya a la pestaña  **Escalado**. 

1. Revise cómo se usa la  **Escala manual** y la **Directiva de reducción horizontal**. 

1. Cambie a una directiva de escalado **Personalizada**. 

1. Revise cómo se usa **Umbral de CPU (%)** para escalar y reducir horizontalmente las instancias de máquina virtual. 

