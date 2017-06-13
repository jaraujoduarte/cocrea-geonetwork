# cocrealabs-geonetwork

El proyecto cocrea-geonetwork tiene como objetivo la automatización del despliegue de la aplicación [Geonetwork](http://geonetwork-opensource.org/) en diferentes tipos de infraestructura destino. Hasta el momento, son soportadas una versión del despliegue para [AWS](https://aws.amazon.com/) y una para [Vagrant/Virtualbox](https://www.vagrantup.com/docs/index.html).

## Arquitectura

La arquitectura propuesta para el despliegue tiene en cuenta los siguientes componentes:

* __Servidor de aplicaciones__ - provee el "runtime" para ejecutar la aplicación Geonetwork

* __Servidor (o servicio en el caso de AWS) de bases de datos__ - a ser usada para persistencia de parte de la mayor parte datos funcionales de Geonetwork

* __Servidor solr__ - alberga solr, un software de indexamiento necesario para la funcionalidad de Geonetwork que permite el [analisis de datos WFS](http://geonetwork-opensource.org/manuals/3.0.5/eng/users/user-guide/analyzing/data.html#analyzing-data)

* __Servidor web__ - sirve como proxy a la infraestructura, exponiendo solo lo necesario para acceder a la aplicación a traves del navegador de manera "segura"

<div style="text-align:center"><img src ="doc/img/infra_general.png" /></div>

## Plataforma destino

Aunque mucho del codigo podria ser reutilizado para lograr un despliegue en gran parte de las distribuciones de Linux, el desarrollo ha sido enfocado y probado solo en maquinas destino __CentOS 7__ y __RedHat Enterprise Linux 7__.

## Tecnologías utilizadas

Las tecnologías utilizadas varian en su naturaleza y aplicación (servidor de aplicaciónes, frameworks, etc), algunas son direcamente la implementación de uno de los componentes detallados en la arquitectura de mas arriba (ej: Apache Tomcat -> Servidor de aplicaciónes) y otras son frameworks o herramientas de apoyo para la automatización y el despliegue de la infraestructura (ej: Ansible -> Automated Configuration Management). A continuación una lista de las tecnologías mas importantes:

* [Apache Tomcat](https://tomcat.apache.org/) - Servidor de aplicaciones
* [Apache Http Server](https://httpd.apache.org/) - Servidor Web
* [Solr](http://lucene.apache.org/solr/) - Software de indexamiento
* [MySQL](https://www.mysql.com/) - Base de datos
* [Ansible](https://www.ansible.com/) - Automated Configuration Management
* [Vagrant](https://www.vagrantup.com/intro/index.html) - Herramienta para la creacion de ambientes virtuales
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) - Software de virtualización

## Como empezar?
Dependiendo de las necesidades se puede hacer uso de una de las versiones del despliegue que han sido implementadas (AWS o Vagrant/Virtualbox). Se debe tener en cuenta que aunque en ambos casos el despliegue lucira como esta detallado arriba (con los diferentes componentes corriendo e interactuando entre sí) la version AWS ha sido implementada para lograr un ambiente de producción y la version basada en Vagrant para realizar pruebas del software o desarrollo.

Para ejecutar cualquiera de las dos versiones implementadas, sigue los siguientes pasos:

1. Sigue las instrucciones mencionadas [aquí](doc/prepare_provisioner.md) para prepara la maquina desde donde se van a ejecutar los scripts - esta maquina sera llamada "Control Machine" a traves de la documentación.

2. Escoge la versión de aprovisionamiento que quieres ejecutar. Para AWS sigue las instrucciones [aquí](doc/aws_provision.md) y para Vagrant sigue las de [aquí](doc/vagrant_provision.md).
