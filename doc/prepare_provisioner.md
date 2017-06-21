# Preparar "Control Machine"

Para poder ejecutar el despliegue automatizado de geonetwork, se debe habilitar una maquina con Ansible (software de "automated configuration management"). Esta maquina la llamaremos "Control Machine" y debe tener instalada una distribución de Linux soportada por Ansible, ademas de poder conectarse a traves de SSH a las maquinas destino de nuestro despliegue.


## Requerimientos

A continuación se explican en mas detalles los requerimientos de la maquina:

* Información actualizada acerca de las distribuciones de Linux soportadas puede ser encontrada [aquí](http://docs.ansible.com/ansible/intro_installation.html#control-machine-requirements).
* Paquetes y software:
  * Python 2.* (por lo general viene en la distribucion de Linux)
  * [Boto](https://github.com/boto/boto#installation)
  * [Ansible 2.2+](http://docs.ansible.com/ansible/intro_installation.html)
* Verificar que la "Control Machine" tiene acceso a las maquinas destino a traves de SSH.

Este repositorio provee scripts para la instalación de los paquetes mencionados arriba y otras dependencias que pueden llegar a ser requeridas. Se proveen scripts para [Ubuntu](../scripts/prepare-provisioner-ubuntu.sh) y para [RedHat Enterprise Linux](../scripts/prepare-provisioner-rhel.sh)
