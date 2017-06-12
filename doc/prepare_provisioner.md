# Preparar "Control Machine"

Para poder ejecutar el despliegue automatizado de geonetwork, se debe habilitar una maquina con Ansible (software de "automated configuration management"). Esta maquina la llamaremos "Control Machine" y debe tener instalada una distribución de Linux soportada por Ansible, ademas de poder conectarse a traves de SSH a las maquinas destino de nuestro despliegue.

A continuación se explican en mas detalles los requerimientos de la maquina y el procedimiento de preparación.


## Requerimientos

* Información actualizada acerca de las distribuciones de Linuz soportadas puede ser encontrada [aquí] (http://docs.ansible.com/ansible/intro_installation.html#control-machine-requirements).
* Paquetes y software:
  * Python 2.* (por lo general viene en la distribucion de Linux)
  * [Boto](https://github.com/boto/boto#installation)
  * [Ansible 2.2+](http://docs.ansible.com/ansible/intro_installation.html)

## Procedimiento

Sigue los siguientes pasos desde la "Control Machine":

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2.
