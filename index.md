---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# <a name="content-directory"></a>Directorio de contenido

Los archivos necesarios para los laboratorios se pueden [DESCARGAR AQUÍ](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

A continuación se enumeran hipervínculos a cada uno de los ejercicios de laboratorio.

## <a name="labs"></a>Laboratorios

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratorio |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


