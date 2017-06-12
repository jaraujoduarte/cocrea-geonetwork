# Aprovisionamiento en Vagrant/Virtualbox
El proyecto entrega una automatización de referencia de un conjunto de maquinas basado en Vagrant y Virtualbox a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

## Requerimientos

* Virtualbox 5.1.22+
* Vagrant 1.9.5+
* Al menos 4GB de RAM
* Al menos 2GB de almacenamiento disponible

## Procedimiento

Una vez instalado todos los requerimientos ve a la terminal y sigue los siguientes pasos:

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Modifica el script de inventario para que sea ejecutable
```
chmod +x $REPO_ROOT/ansible/inventory/aws.yml
```
3. Copia la parte privada de la llave ssh especificada en el archivo aws.yml en el folder "$REPO_ROOT/sshkey/millave.pem"

4. En el archivo "$REPO_ROOT/ansible/aws.yml", agrega tus credenciales para hacer uso de la API de AWS
```
...
access_key: "TU_ACCESS_KEY"
secret_key: "TU_SECRET_KEY"
...
```

## Solución de problemas
