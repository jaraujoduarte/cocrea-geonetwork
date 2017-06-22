## Modificaciones a Archivos de Variables
Para realizar modificaciones a los archivos de variables usados por los roles de Ansible que realizan el aprovisionamiento de la infraestructura consulte los siquientes archivos:
- Instancias EC2 AWS -> [ansible/aws/group_vars/all](../ansible/aws/group_vars/all)
- Bases de datos AWS -> [ansible/aws/group_vars/all](../ansible/aws/group_vars/all)
- Bases de datos local -> [ansible/group_vars/tag_Name_dbserver](../ansible/group_vars/tag_Name_dbserver)
- Servidor de aplicaciones -> [ansible/group_vars/tag_Name_appserver](../ansible/group_vars/tag_Name_appserver)
- Servidor Web -> [ansible/group_vars/tag_Name_webserver](../ansible/group_vars/tag_Name_webserver)
- Servidor Solr -> [ansible/group_vars/tag_Name_solrserver](../ansible/group_vars/tag_Name_solrserver)
