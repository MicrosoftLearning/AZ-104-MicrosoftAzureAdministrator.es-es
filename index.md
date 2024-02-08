---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# Directorio de contenido

Los archivos necesarios para los laboratorios se pueden [DESCARGAR AQUÍ](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

## Laboratorios

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratorio |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

## Demostraciones

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Módulo | Demostración |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
