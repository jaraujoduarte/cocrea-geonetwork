# Aprovisionamiento en AWS
El proyecto entrega una automatización de referencia de un conjunto de maquinas en AWS a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

## Requerimientos

* Una maquina con una distribución de Linux soportada por  Ansible con acceso a internet [(mas info aquí)](http://docs.ansible.com/ansible/intro_installation.html#control-machine-requirements). Esta maquina sera usada como "Control Machine".
* En la "Control Machine":
  * Python 2.*
  * Boto (Paquete de python)
  * Ansible 2.2+

## Procedimiento

Sigue los siguientes pasos desde la "Control Machine":

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
