---
demo:
  title: "Demostración\_06: Administración del tráfico de red"
  module: Administer Network Traffic Management
---


# 06: Administración del tráfico de red

## Configuración de Azure Load Balancer

En esta demostración aprenderá a crear un equilibrador de carga público. 

**Nota:**  Para esta demostración se necesita una red virtual con al menos una subred. 

**Referencia**: [Inicio rápido: Uso de Azure Portal para crear un equilibrador de carga público para equilibrar la carga de máquinas virtuales](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal).

**Mostrar la característica “Ayudarme a elegir” del portal**

1. Acceda a Azure Portal.

1. Busque y seleccione **Equilibrio de carga: ayuda para elegir**.

1. Use el asistente para recorrer diferentes escenarios.
   
**Creación de un equilibrador de carga**

1. Continúe en Azure Portal.

1. Busque y seleccione **Equilibrador de carga**. **Cree** un equilibrador de carga. 

1. En la pestaña **Datos básicos**, analice los valores de **SKU**, **Tipo** y **Nivel**.

1. En la pestaña **Configuración de IP de front-end**, analice el uso de una dirección IP pública.

1. En la pestaña **Grupos de back-end**, seleccione la red virtual con un intervalo de direcciones IP.

1. En la pestaña **Reglas de entrada**, cree una regla de equilibrio de carga. Analice parámetros como **Protocolo**, **Puertos**, **Sondeos de estado** y **Persistencia de la sesión**. 


## Configuración de Azure Application Gateway

En esta demostración aprenderá a crear una instancia de Azure Application Gateway. 

**Nota:** Para hacerlo más sencillo, cree nuevas redes virtuales y subredes a medida que defina la configuración. 

**Referencia**: [Inicio rápido: Dirección del tráfico web con Azure Application Gateway: Azure Portal](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Crear la instancia de Azure Application Gateway**

1. Acceda a Azure Portal.

1. Busque y seleccione **Azure Application Gateway**.

1. **Cree** una puerta de enlace.

1. En la pestaña **Datos básicos**, analice los parámetros **Niveles**, **Escalado automático** y **Recuento de instancias**.

1. En la pestaña **Front-end**, analice los tipos de dirección IP.

1. En la pestaña **Back-end**, analice cuándo debe usarse un grupo de back-end vacío.

1. En la pestaña **Configuración**, analice las reglas de enrutamiento. Compárelas con las reglas del equilibrador de carga.

1. Explique que, después de crear la puerta de enlace, agregaría destinos de back-end y haría pruebas. 
