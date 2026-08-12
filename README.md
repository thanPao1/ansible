# Playbooks
## Ansible
### Objetivo:
Que Ansible ejecute un job/playbook que invoque a BMC Discovery para realizar un discovery inmediato sobre una IP.

### REST API:
BMC Discovery expone APIs para:
  * crear scans
  * ejecutar discovery runs
  * consultar resultados

Ansible consumiría esas APIs.

### Infraestructura Discovery:
Contamos con 11 Outpost (Servidores windows Onprem donde se almacenan las credenciales y donde discovery se comunica con los destinos)

Operación que usarias:
Adhoc Discovery ó Scan Range API

Ansible:
Ansible NO necesita:
  
  * SSH al Outpost 
  * Root en appliance
  * Shell administrativo

Solo necesita:

acceso HTTPS/API a Discovery

Para ello necessitas que tu playbook tenga comunicación a https://liverpool-itom.onbmc.com/ por el 443

Usuario en ADDM:
Mínimos necesarios:
Debe poder:

Ejecutar discovery scans
Consultar resultados
Acceder a APIs
Leer ranges/outposts

El cual los roles serian:
* Discovery User
* API Access
* Scan Control / Discovery Run permissions

### Grupo en ADDM:
 api_users ó discovery_operators... Para esto yo puedo crear un grupo que se llame "ansible_discovery_automation" para tener bien mapeado el uso de ese usuario

### SHELL:
 No lo necesitas porque solo se usara API ya que no requieren login a linux

## Nagios
Estructura de playbook y recursos necesarios

### IP: 
* 172.16.202.219
* User: ansible_nagios
* API_Token: rCLns0G8Z9B06kGXG7EqaeL5GRGGJ8m64P40K4mCjPHXmlDZcb3BENV8TWhgP9JQ

La estructura para dar de alta los hots, es la siguiente: 

```
define host {
    host_name                qroplantso8_copy_1
    use                      liv-host-linux
    alias                    Plantilla
    address                  172.16.202.43
    hostgroups               linux-servers_base
    contacts                 nagiosadmin
    notifications_enabled    0
    register                 1
}
```

Es importante mencionar que la playbook no debe de crear los servicios de monitoreo, únicamente dar de alta el Host en el hostgroup antes mencionado.
