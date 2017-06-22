# Aprovisionamiento en Vagrant/Virtualbox
El proyecto entrega una automatización de referencia de un conjunto de maquinas basado en Vagrant y Virtualbox a ser usadas para desplegar los diferentes componentes de la instalación de Geonetwork.

## Requerimientos

* Virtualbox 5.1.22+
* Vagrant 1.9.5+
* Git 2.13.1+ (Si se ejecuta desde Windows - Git Bash)
* Al menos 8GB de RAM
* Al menos 2GB de almacenamiento disponible
* Conexión a internet

## Procedimiento

1. Descarga el repositorio
```
git clone https://github.com/jaraujoduarte/cocrea-geonetwork.git
```

2. Ejecute el despliegue de la infraestructura
```
# $REPO_ROOT
cd vagrant
vagrant up
```

3. Acceda a Geonetwork a traves de la URL https://10.0.0.23/geonetwork

## Modificaciones
Para modificaciones mas alla de las que se pueden proveer por medio de input.yml, consulte [aquí](modifications.md)
