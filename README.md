# Playbooks
# Ejecución
## Playbook 1 
Entorno de ejecución - MV gcpagtama00 

Se requiere IP a descubrir
`ansible-playbook bmc-discovery-main.yml -e "target_ip=0.0.0.0"`

## Playbook 2
Entorno de ejecución - AAP

Se requiere:
* Host name
* Alias del host
* IP 

Job que se ejecuta `Nagios | Alta de Host`

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
11 Outpost (Servidores windows Onprem donde se almacenan las credenciales y donde discovery se comunica con los destinos)

Operación que se usa:
Adhoc Discovery 

Ansible:
Ansible NO necesita:
  
  * SSH al Outpost 
  * Root en appliance
  * Shell administrativo

Solo necesita acceso HTTPS/API a Discovery

Para ello este playbook cuenta con comunicación a https://liverpool-itom.onbmc.com/ mediante el puerto 443

Usuario en ADDM:
* Mínimos necesarios:

Debe poder:

* Ejecutar discovery scans
* Consultar resultados
* Acceder a APIs
* Leer ranges/outposts

El cual los roles serian:
* Discovery User
* API Access
* Scan Control / Discovery Run permissions

Grupo en ADDM:
ansible_discovery_automation

SHELL:
No, solo API

## Nagios
Estructura de playbook y recursos necesarios

IP: `172.16.202.219`
User: `ansible_nagios`
API_Token

Estructura para dar de alta los hots:

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
Es importante mencionar que la playbook no crea los servicios de monitoreo, únicamente dar de alta el Host en el hostgroup antes mencionado.
