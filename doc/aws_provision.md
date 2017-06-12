# Aprovisionamiento en AWS
El proyecto entrega una automatización de referencia de un conjunto de maquinas en AWS a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

En esta versión del despliegue, se utiliza el servicio [EC2](https://aws.amazon.com/ec2/) (Elastic Compute Cloud) para la provisión de las maquinas virtuales donde van a correr nuestros servicios, y [RDS](https://aws.amazon.com/rds/) (Relational Database Service) a traves de la cual se instancia una base de datos relacional de Geonetwork.

## Requerimientos

* Una "[Control Machine](prepare_provisioner.md)" desde donde correr los playbooks de Ansible.

* "Access Key" y "Secret Key" de AWS con los suficientes permisos para utilizar las APIs de los services EC2 y RDS. Instrucciones detalladas pueden ser encontradas aquí [aquí](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey).

* Un par de llaves (privada y publica) SSH que esten registradas en la zona de disponibilidad en las cual se vayan a desplegar las maquinas virtuales (por ejemplo: London/eu-west-2). Instrucciones para la creación de estas llaves pueden ser encontradas [aquí](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).

## Procedimiento

Sigue los siguientes pasos desde la "Control Machine":

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Modifica el script de inventario para que sea ejecutable
```
chmod +x $REPO_ROOT/ansible/inventory/ec2.py
```

3. Copia la parte privada de la llave ssh especificada en el directorio "$REPO_ROOT/sshkey/geokey.pem"

4. Agrega tus credenciales para hacer uso de la API de AWS
```
# $REPO_ROOT/ansible/aws/all
...
access_key: "TU_ACCESS_KEY"
secret_key: "TU_SECRET_KEY"
...
```

5. AWS crea una VPC por defecto a la cual son añadidas las maquinas virtuales instanciadas por EC2. Obten el id de esta VPC y modificalo en el archivo de variables.
```
# $REPO_ROOT/ansible/aws/all
...
m_vpc_id: vpc-elidcorrecto
...
```

6. Aunque con los cambios hechos hasta ahora podrias ejecutar un despliegue, la especificaciónes de las maquinas virtuales no corresponden a un ambiente de producción. Puedes cambiar las espcificaciónes y la imagen (AMI) de la maquina virtual siempre y cuando correspondan a un sistema __CentOS 7__ o __RedHat Enterprise Linux 7__. Tambien pueden ser modificadas las espcificaciones de las base de datos siempre y cuando correspondan a una base de datos MySQL.
```
# $REPO_ROOT/ansible/aws/all
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

## Solución de problemas
A continuación hay una lista de los posibles problemas junto con soluciones, que podras encontrar al ejecutar el aprovisionamiento.

<strong>EC2ResponseError: 401 Unauthorized<strong>

Empieza por verifica que tu access key y secret. Si no has especificado esas entradas de manera correcto, prosigue a
verificar que la hora y la fecha esten sincronizadas - debes ejecutar el siguiente comando desde la "Control Machine":

```
sudo ntpdate time.apple.com
```

<strong>Permissions 0644 for 'sshkey/millave.pem' are too open<strong>

Modifica los permisos para el archivo que contiene tu llave:

```
sudo chmod 600 sshkey/millave.pem
```
