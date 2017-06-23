# Ansible
## Definici髇 
Ansible es una herramienta que permite automatizar todas las tediosas y repetitivas tareas que se pueden generar al administrar un sistema.
## Funcionamiento
Una de las grandes ventajas frente a las competencias es que no es necesario instalar agentes ni servidores. Simplemente instalar ansible en la maquina deseada.
Ansible maneja una machine controler y diferentes nodos. Machine controler se conectara a los nodos por medio de SSH para enviar las instrucciones y configuraciones a ejecutar en los nodos deseados.
![Funcionamiento ejemplo](img/ansible-work.png)
## Rol en geonetwork
Geonetwork esta siendo despleglado dentro de una maquina virtual y la manera en la cual se esta llegando a ella es por medio de ansible creando task programadas en __YAML__ haciendo uso de los modulos necesarios.
## Configuraciones
Para realizar las correctas configuraciones es necesario leer la documentacion de __vagrant__, ya que se esta realizando un aprovisionamiento __Vagrant Ansible__. Las configuraciones mas directas con Ansible son realizadas en el archivo llamado ansible.cfg.
### Ansible.cfg
Este archivo guarda la configuraci贸n general de ansible. Las configuraciones aplicadas para este caso son descritas a continuaci贸n.
En este caso se deshabilita totalmente la verificaci贸n del host SSH clave.
```javascript
[defaults]
host_key_checking = no
```
El anterior paramentro indica la comprobaci贸n de clave host, la cual se encuentra activa en Ansible 1.3 y posteriores, por tal razon se desactiva.
```javascript
[ssh_connection]
ssh_args = -o ControlPersist=60s -o UserKnownHostsFile=/dev/null -o IdentitiesOnly=yes
```
Los ajustes anteriores son especificamente para conexiones SSH, al establecer __**ssh_args**__ pasara a un conjunto especifico de opciones que se le estan dando a continuacion como se observa __ControlPersist=60s..__ de modo que no tomara los valores predeterminados.
```javascript
ControlPersist=60s
```
ControlPersist nos permite definir el tiempo durante el cual se mantendr谩 viva la conexi贸n original una vez se haya cerrado (en caso que queden tareas pendientes en alguna de las sesiones secundarias).
### Inventario
Ansible trabaja con un inventario, este inventario es un archivo en el cual se agrupan y enlistan los nodos segun mas convenga.
A continuaci贸n se dara un breve explicaci贸n del por que se encuentran los parametros actualmente.
```
manager ansible_connection=local
```
Ansible no se limita a un tipo de conexion, con este parametro especifico de host el tipo de conexion se puede cambiar. En este caso se utiliza local ya que es necesario desplegar el libro de jugadas a la machine control de si mismo.
```javascript
appserver ansible_user=vagrant ansible_host=10.0.0.21 ansible_ssh_private_key_file=/keys/private_key_appserver
```
De la manera mostrada anteriormente se agrega un host al inventario y es posible ejecutar las tareas ya programadas sobre ese host.
Este ejemplo contiene una serie de parametros, los cuales seran descritos a continuaci贸n:
* ansible_user: El nombre de usuario para poder operarlo.
* ansible_host: El nombre del nodo con el cual sera identificado.
* ansible_ssh_private_key_file: Este archivo es la llave con la cual es se conectara por SSH manejado.

Los anteriores parametros indican con que nombre, host y clave se conectara al servidor.
```
[tag_Name_appserver]
appserver
```
Los servidores o nodos pueden ser agrupados por caracteristicas que sean consideradas importantes para manejar un mayor orden y no tener la necesidad de llamar a cada uno.
## Vagrant
### Definicion
Vagrant es una herramienta para la creaci贸n y configuraci贸n de entornos de desarrollo virtualizados. Brinda la capacidad de instalar y configurar software en una maquina virtual para simular que nos encontramos en el servidor en el que se alojara la aplicaci贸n web.
### Rol en geonetwork
Gracias a las capacidades de vagrant, se esta haciendo un aprovisionamiento ``` Vagrant-Ansible``` 
de modo que con vagrant y virtualbox se esta creando un entorno de desarrollo haciendo uso de una de sus box llamada ``` ubuntu/trusty64```  en la cual se esta aprovisionando ansible y desde la cual se realiza el despliegue a los servidores.

Del mismo modo se crean tres mas con la diferencia que estan no utilizan la box ``` ubuntu/trusry64```  ni tampoco aprovisionan a Ansible. Estas maquinas manejan la caja ``` centos/7```  y aca una de ellas es agregada un ip con la cual seran identificadas.
* appserver
* solrserver
* webserver

Ejemplo creacion y configuracion de appserver.
```javascript
config.vm.define "appserver" do |appserver|
		appserver.vm.provider :virtualbox do |vb|
			vb.memory = 1024
	    vb.cpus = 2
		end
    appserver.vm.network "private_network", ip: "10.0.0.21"
  end
```
### Configuracion despues de instalarlo
Una vez instalado *Vagrant* junto con *Virtualbox* (si no se tiene instalado ya), es necesario crear una carpeta y seguidamente desde CMD y dentro de la carpeta ejecutar el comando ```vagrant init```, este comando crear una carpeta llamada *.vagrant* la cual contendra las llaves SSH una vez se cree una machine y un archivo de configuracion llamado *Vagrantfile* en el cual debe ir las configuraciones correspondientes que se encuentran en la siguiente [ruta](https://github.com/FelipeMora/cocrea-geonetwork/blob/docs/vagrant/Vagrantfile).
Para que las configuraciones del archivo Vagrantfile se ejecuten es necesario ejecutar ```vagrant up```  el cual creara los entornos de desarrollo planteados en el archivo.
### Cambios que se pueden hacer antes de desplegar geonetwork
Modificar las configuraciones de vagrant en ocasiones pueden implicar llegar a mas de un archivo.
#### Memoria y cpu asignada 
Cambiar la memoria asignada a un servidor se realiza desde el archivo ```Vagrantfile```, especificamente en esta linea.
```javascript
vb.memory = 1024
```
La memoria asignada a cada servidor es de 1024 megabytes que equivale a 1,024 gigabytes siendo esto considerado lo necesario, pero en caso de querer requerer mas por diferentes motivos y la maquina dispone de tal capacidad es posible hacerlo.
### Configuraci髇 de red de los servidores
Las configuraciones de la tarjeta de red en modo _Red interna_ de ```VirtualBox``` se estan realizando de la siguiente manera.
```
appserver.vm.network "private_network", ip: "10.0.0.21"
``` 
En donde se esta indicando donde se va a poner la ip que va a tener la maquina.
## VirtualBox
### Definicion
VirtualBox es una herramienta de virtualizaci髇 de codigo abierto multiplataforma, que permite crear unidades de disco virtuales en donde se puede instalar un sistema operativo invitado dentro del que se utiliza normalmente en el equipo.
### Rol en geonetwork
VirtualBox es el proveedor que se esta manejando para Vagrant, de tal forma que los servidores que se encuentran en el archivo ```Vagrantfile``` virtualbox es el encargado de crearlos.