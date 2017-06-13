# Aprovisionamiento en AWS
El proyecto entrega una automatización de referencia de un conjunto de maquinas en AWS a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

En esta versión del despliegue, se utiliza el servicio [EC2](https://aws.amazon.com/ec2/) (Elastic Compute Cloud) para la provisión de las maquinas virtuales donde van a correr nuestros servicios, y [RDS](https://aws.amazon.com/rds/) (Relational Database Service) a traves de la cual se instancia la base de datos relacional de Geonetwork.

## Requerimientos

* Una "[Control Machine](prepare_provisioner.md)" desde donde correr los playbooks de Ansible.

* "Access Key" y "Secret Key" de AWS con los suficientes permisos para utilizar las APIs de los services EC2 y RDS. Instrucciones detalladas pueden ser encontradas [aquí](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey).

* Un par de llaves (privada y publica) SSH que esten registradas en la zona de disponibilidad en las cual se vayan a desplegar las maquinas virtuales (por ejemplo: London/eu-west-2). Instrucciones para la creación de estas llaves pueden ser encontradas [aquí](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).


## Procedimiento

Sigue los siguientes pasos desde la "Control Machine":

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Modifica el script de inventario para que sea ejecutable
```
chmod +x $REPO_ROOT/ansible/aws/inventory/ec2.py
```

3. Copia la parte privada de la llave ssh especificada en el directorio "$REPO_ROOT/sshkey/geokey.pem"

4. Especifica la ruta hacia la llave ssh privada
```
# $REPO_ROOT/ansible/group_vars/ec2
...
ansible_ssh_private_key_file: "/my/path/cocrea-geonetwork/ansible/aws/sshkey/geokey.pem"
```

5. Agrega tus credenciales para hacer uso de la API de AWS
```
# $REPO_ROOT/ansible/aws/group_vars/all
...
access_key: "TU_ACCESS_KEY"
secret_key: "TU_SECRET_KEY"
...
```

6. AWS crea una VPC por defecto a la cual son añadidas las maquinas virtuales instanciadas por EC2. Obten el id de esta VPC y modificalo en el archivo de variables.
```
# $REPO_ROOT/ansible/aws/group_vars/all
...
m_vpc_id: vpc-elidcorrecto
...
```

7. Aunque con los cambios hechos hasta ahora podrias ejecutar un despliegue, la especificaciónes de las maquinas virtuales no corresponden a un ambiente de producción. Puedes cambiar las espcificaciónes y la imagen (AMI) de la maquina virtual siempre y cuando correspondan a un sistema __CentOS 7__ o __RedHat Enterprise Linux 7__. Tambien pueden ser modificadas las espcificaciones de las base de datos siempre y cuando correspondan a una base de datos MySQL.
```
# $REPO_ROOT/ansible/aws/group_vars/all
...
ec2_instance_type: t2.micro
ec2_image: ami-9c363cf8
db_instance_type: db.t2.micro
db_size: 10
db_instance_name: default-instance
db_db_name: geonetworkdb
db_username: defaultuser
db_password: default1
...
```

8. Ejecuta la creación de la instancia de la base de datos
```
cd $REPO_ROOT/ansible/aws/
ansible-playbook -v rds.yml
```

9. Ejecuta la creación de las maquinas virtuales
```
cd $REPO_ROOT/ansible/aws/
ansible-playbook -v ec2.yml
```

10. Modifica las variables usadas como parametros de entrada para los playbooks de Ansible. Información detallada acerca de los valores [aquí]()

11. Ejecuta el despliegue de los Servidores de Aplicaciones, Web y Solr haciendo uso del inventario dinamico de Ansible para AWS
```
cd $REPO_ROOT/ansible/
ansible-playbook -i ../aws/invetory/ec2.py -v solr.yml
ansible-playbook -i ../aws/invetory/ec2.py -v appserver.yml
ansible-playbook -i ../aws/invetory/ec2.py -v webserver.yml
```

## VPC y Security Groups

Los scripts que despliegan la infrastructura en AWS crean los siguientes "[Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)" para configurar el acceso a la infraestructura desplegada:

* __default__ - acceso a todos los puertos para maquinas en la misma VPC
* __geonetwork-default__ - acceso SSH desde fuera de la VPC
* __geonetwork-webserver__ - acceso HTTP/443 desde fuera de la VPC (utilizado para acceder al Servidor Web)


## Solución de problemas
A continuación hay una lista de los posibles problemas junto con soluciones, que podras encontrar al ejecutar el aprovisionamiento.

__EC2ResponseError: 401 Unauthorized__

Empieza por verifica que tu access key y secret. Si no has especificado esas entradas de manera correcto, prosigue a
verificar que la hora y la fecha esten sincronizadas - debes ejecutar el siguiente comando desde la "Control Machine":

```
sudo ntpdate time.apple.com
```

__Permissions 0644 for 'sshkey/millave.pem' are too open__

Modifica los permisos para el archivo que contiene tu llave:

```
sudo chmod 600 sshkey/millave.pem
```

__boto required for this module__

Verifica que el modulo "boto" de Python este instalado
