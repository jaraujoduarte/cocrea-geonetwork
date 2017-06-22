# Aprovisionamiento en AWS (Simple)
El proyecto entrega una automatización de referencia de un conjunto de maquinas en AWS a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

En esta versión del despliegue, se utiliza el servicio [EC2](https://aws.amazon.com/ec2/) (Elastic Compute Cloud) para la provisión de las maquinas virtuales donde van a correr nuestros servicios, y [RDS](https://aws.amazon.com/rds/) (Relational Database Service) a traves de la cual se instancia la base de datos relacional de Geonetwork.

## Requerimientos

* Virtualbox 5.1.22+
* Vagrant 1.9.5+
* Git 2.13.1+ (Si se ejecuta desde Windows - Git Bash)
* Al menos 4GB de RAM
* Al menos 1GB de almacenamiento disponible

## Notas
Antes de empezar lea con atención la siguiente información:
- Asegurese que no haya un "Key Pair" con el nombre "geonetwork" en el "Availability Zone" de AWS que este trabajando.
- Asegurese que el directorio $REPO_ROOT/ansible/aws/sshkey no existe o esta vacío.
- El tipo de maquinas solicitadas es t2.small debido a los requerimientos minimos de Geonetwork, las cuales no hacen parte del "Free tier" de AWS.
- La creación de las instancias EC2 no es un proceso idempotente, así que cada vez que se ejecute el despliegue nuevas instancias serán creadas. Asegurese de remover instancias EC2 innecesarias.

## Procedimiento

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Provee las credenciales para la API de AWS
```
# $REPO_ROOT/vagrant/input.yml
...
i_aws_secret_access_key: "TU_SECRET_ACCESS_KEY"
i_aws_access_key_id: "TU_ACCESS_KEY_ID"
...
```

3. Ejecute el despliegue de la instancia de la base de datos. Vagrant instancia una maquina virtual desde donde correr los playbooks de Ansible. Por este motivo puede que se muestren dialogos de confirmación cuando Virtualbox este creando la maquina.

  Si es la primera vez que se ejecuta (no existe la maquina virtual):
```
# $REPO_ROOT
cd vagrant
vagrant --aws --db up
```

  Si no es la primera vez que se ejecuta (existe la maquina virtual) debido a un error previo u otro motivo:
```
# $REPO_ROOT
cd vagrant
vagrant --aws --db provision
```
La instancia puede demorar alrededor de 5 minutos para estar disponible.

4. Verifique el endpoint de la instancia de la base de datos en la consola de AWS. Una vez hecho esto, modifica el parametro de entrada en archivo input.yml (sin el puerto! - vea la seccion de modificaciones en caso de que sea necesario cambiar el puerto por defecto):
```
# $REPO_ROOT/vagrant/input.yml
...
i_db_endpoint: "tu-enpointr.rds.amazonaws.com"
...
```

5. Ejecute el despliegue del resto de la infraestructura
```
# $REPO_ROOT
cd vagrant
vagrant --aws provision
```

6. Acceda Geonetwork a traves del dominio publico (public DNS) del Servidor Web (aparece como webserver) en la consola, y acceda usando https (i.e. https://mi-dominio-public-aws.eu-west-2.compute.amazonaws.com/geonetwork).

## VPC y Security Groups

Los scripts que despliegan la infrastructura en AWS crean los siguientes "[Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)" para configurar el acceso a la infraestructura desplegada:

* __default__ - acceso a todos los puertos para maquinas en la misma VPC
* __geonetwork-default__ - acceso SSH desde fuera de la VPC
* __geonetwork-webserver__ - acceso HTTP/443 desde fuera de la VPC (utilizado para acceder al Servidor Web)

## Modificaciones
Para modificaciones mas alla de las que se pueden proveer por medio de input.yml, consulte [aquí](modifications.md)
