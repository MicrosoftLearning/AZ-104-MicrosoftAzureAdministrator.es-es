---
demo:
  title: "Demostración\_04: Administración de redes virtuales"
  module: Administer Virtual Networking
---

# 04: Administración de redes virtuales

## Configuración de redes virtuales

En esta demostración, creará redes virtuales.

**Referencia**: [Inicio rápido: Uso de Azure Portal para crear una red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## Creación de una red virtual en el portal

1.  Inicie sesión en Azure Portal y busque  **Redes virtuales**.

1.  Cree una red virtual y explique la configuración básica a medida que avance. Asegúrese de que se crea al menos una subred. 

1.  Compruebe que se ha creado la red virtual.

## Configurar grupos de seguridad de redes

En esta demostración, explorará los NSG y los puntos de conexión de servicio.

**Referencia**: [Restricción del acceso a los recursos de PaaS (tutorial): Azure Portal](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**Crear un grupo de seguridad de red**

1. Acceda a Azure Portal.

1. Busque y seleccione  **Grupos de seguridad de red**.

1. Cree un grupo de seguridad de red para explicar la configuración a medida que avance. 
 
1. Espere a que se implemente el nuevo grupo de seguridad de red.

**Exploración de reglas de entrada y salida**

1. Seleccione el nuevo grupo de seguridad de red.

1. Analice cómo se puede asociar el grupo de seguridad de red a subredes o interfaces de red.

1. Analice el propósito de las reglas de entrada y salida.  

1. Revise las reglas de entrada y salida predeterminadas. 

1. Cree una regla para explicar la configuración a medida que avance. En concreto, analice la selección del servicio (como HTTPS) y la configuración de prioridad. 

## Configuración de Azure DNS

En esta demostración, se explorará Azure DNS.

**Referencia**: [Tutorial: Hospedaje del dominio y los subdominios en Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Creación de una zona DNS**

1. Acceda a Azure Portal.

1. Busque el servicio  **Zonas DNS**.

1. Cree una **Zona DNS** y explique el propósito de la zona. Para el nombre, puede usar contoso.internal.com.

1.  Espere a que se cree la zona DNS. Es posible que tenga que  **Actualizar** la página.

**Adición de un conjunto de registros de DNS**

**Referencia**: [Tutorial: Creación de un registro de alias para hacer referencia a un registro de recursos de zona](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Una vez creada la zona, seleccione  **+Conjunto de registros**.

1. Use la lista desplegable  **Tipo** para ver los distintos tipos de registros. Revise cómo se usan los distintos tipos de registros. Observe cómo cambia la información del registro a medida que selecciona los diferentes tipos de registros.

1. Cree un registro **A** como ejemplo. 

