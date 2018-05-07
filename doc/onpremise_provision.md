
# Aprovisionamiento On-Premise (Manual)

En esta versión del despliegue, se utilizan maquina On-Premise y aplica para instalación en un unico nodo o instalaciones multi-nodo.

## Requerimientos

* Ansible

* Si es una instalción multi-nodo, usuario creado en las maquinas destino para acceso con SSH con escalación a root configurada a través de "su"

* Maquinas destino deben ser CentOS o RHEL

* Conexión a internet en los nodos destino


## Preparar "Control Machine"

Para poder ejecutar el despliegue automatizado de geonetwork, se debe habilitar una maquina con Ansible (software de "automated configuration management"). Esta maquina la llamaremos "Control Machine" y debe tener instalada una distribución de Linux soportada por Ansible.


A continuación se explican en mas detalles los requerimientos de la maquina:

* Información actualizada acerca de las distribuciones de Linux soportadas puede ser encontrada [aquí](http://docs.ansible.com/ansible/intro_installation.html#control-machine-requirements).
* Paquetes y software:
  * Python 2.* (por lo general viene en la distribución de Linux)
  * [Ansible 2.2+](http://docs.ansible.com/ansible/intro_installation.html)
* Verificar que la "Control Machine" tiene acceso a las maquinas destino a traves de SSH (en caso de instalación multi-nodo o si la Control Machine no es el mismo nodo-unico).

Este repositorio provee scripts para la instalación de los paquetes mencionados arriba y otras dependencias que pueden llegar a ser requeridas. Se proveen scripts para [Ubuntu](../scripts/prepare-provisioner-ubuntu.sh) y para [RedHat Enterprise Linux](../scripts/prepare-provisioner-rhel.sh).

**Nota:** Aunque la Control Machine puede tener cualquier OS soportado por Ansible, las maquinas destino deben ser RHEL o CentOS.

**Nota:** En caso de realizar la instalción en un solo nodo, el unico nodo/maquina puede ser unado también como Control Machine (siempre y cuando sea RHEL o CentOS).

## Procedimiento

Sigue los siguientes pasos desde la "Control Machine":

1. Descarga el repositorio
```shell
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Edita el archivo de inventario para Ansible si es necesario (ej: Si es una instalación multi-nodo), incluyendo los argumentos de la conexión de Ansible a tu(s) maquina(s) destino.
```shell
$REPO_ROOT/ansible/onpremise/inventory.ini
```

3. Edita los argumentos de descarga de los paquetes de Tomcat y Geonetwork:
```yaml
$REPO_ROOT/ansible/group_vars/all.yml

# Default download information
def_tomcat_pkg_url: "http://mirrors.supportex.net/apache/tomcat/tomcat-7/v7.0.86/bin/apache-tomcat-7.0.86.tar.gz"
def_tomcat_pkg_version: "apache-tomcat-7.0.86"
def_geonetwork_pkg_url: "http://datapacket.dl.sourceforge.net/project/geonetwork/GeoNetwork_opensource/v3.4.2/geonetwork.war"
```

4. Modifica las variables usadas como parametros de entrada para los playbooks de Ansible (en caso de ser necesario) [aquí](modifications.md).

5. Ejecuta el despliegue de los Servidores de Aplicaciones, Web y Solr haciendo uso del inventario:
```shell
cd $REPO_ROOT/ansible/
ansible-playbook -i ../onpremise/invetory.ini -v allin_local.yml
```

6. Suponiendo que "192.168.1.2" fuera la ip del Servidor Web, acceda a Geonetwork a traves de la URL https://192.168.1.2/geonetwork

## Solución de problemas
A continuación hay una lista de los posibles problemas junto con soluciones, que podras encontrar al ejecutar el aprovisionamiento.

__Permissions 0644 for 'sshkey/millave.pem' are too open__

Modifica los permisos para el archivo que contiene tu llave:

```shell
sudo chmod 600 sshkey/millave.pem
```