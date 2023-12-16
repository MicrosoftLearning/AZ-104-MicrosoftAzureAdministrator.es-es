---
demo:
  title: "Demostración\_05: Administración de la conectividad entre sitios"
  module: Administer Intersite Connectivity
---

# 05: Administración de la conectividad entre sitios

## Configuración del emparejamiento de VNet

**Nota:**  Para esta demostración necesitará dos redes virtuales.

**Referencia**: [Tutorial sobre conexión de redes virtuales con emparejamiento de VNet](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configuración del emparejamiento de VNet en la primera red virtual**

1. En  **Azure Portal**, seleccione la primera red virtual. Revise el valor del emparejamiento. 

1. En  **Configuración**, seleccione  **Emparejamientos** y **+ Agregar** un nuevo emparejamiento.

1. Configure el emparejamiento de la segunda red virtual. Use los iconos de información para revisar la configuración diferente. 

1. Una vez completado el emparejamiento, revise el **Estado de emparejamiento**. 

**Confirmación del emparejamiento de VNet en la segunda red virtual**

1. En  **Azure Portal**, seleccione la segunda red virtual.

1. En  **Configuración**, seleccione  **Emparejamientos**.

1. Observe que se ha creado un emparejamiento de forma automática. Observe que el  **Estado de emparejamiento** es  **Conectado**.

1. En el laboratorio, los alumnos crearán el emparejamiento y probarán la conexión entre las máquinas virtuales. 

## Configuración de enrutamiento de red y de puntos de conexión

En esta demostración, aprenderá a crear una tabla de rutas, a definir una ruta personalizada y a asociarla a una subred.

**Nota:**  Para esta demostración se necesita una red virtual con al menos una subred.

**Referencia**: [Enrutamiento del tráfico de red (tutorial): Azure Portal](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Creación de una tabla de enrutamiento**

1. Como tiene tiempo, revise el diagrama del tutorial. Explique por qué necesita crear una ruta definida por el usuario. 

1. Acceda a Azure Portal.

1. Busque y seleccione **Tablas de rutas**. Analice cuándo se deben usar las **rutas de puerta de enlace propagación**. 

1. Cree una tabla de enrutamiento y explique cualquier configuración poco común. 

1. Espere a que se implemente la nueva tabla de rutas.

**Agregar una ruta**

1.  Seleccione la nueva tabla de enrutamiento y después  **Rutas**.

1.  Cree una **ruta**. Analice los diferentes **tipos de salto** que están disponibles. 

1.  Cree la ruta y espere a que el recurso se implemente.
 
**Asociación de una tabla de rutas a una subred**

1.  Vaya a la subred que quiera asociar a la tabla de rutas.

1.  Seleccione  **Tabla de rutas** y elija la nueva tabla de rutas. 

1.  **Guarde** los cambios.

