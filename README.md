# Ansible deployment of a icinga2 cluster

Demonstrator um verschiedene Szenarien zu testen und die Entwicklung der ansible-icinga2 Rolle zu testen.

## usage

download external ansible roles

```
$ ansible-galaxy install -r requirements.yml [--force]
```

## DNS

`/etc/hosts`

```
192.168.130.10          database-1.icinga.local
192.168.130.12          tsdb-1.icinga.local
192.168.130.15          icingaweb.icinga.local
192.168.130.20          master-1.icinga.local
192.168.130.21          master-2.icinga.local
192.168.130.30          satellite-1.icinga.local
192.168.130.31          satellite-2.icinga.local
192.168.130.32          satellite-3.icinga.local
192.168.130.33          satellite-4.icinga.local
192.168.130.34          satellite-5.icinga.local
192.168.130.35          satellite-6.icinga.local
192.168.130.36          satellite-7.icinga.local
192.168.130.37          satellite-8.icinga.local
192.168.130.38          satellite-9.icinga.local
192.168.130.39          satellite-10.icinga.local
```

## vault support

```
openssl rand -base64 2048 > ansible-vault.pass

ansible-vault create host_vars/monitoring.cm.local/vault --vault-password-file=host_vars/monitoring.cm.local/ansible-vault.pass
ansible-vault edit   host_vars/monitoring.cm.local/vault --vault-password-file=host_vars/monitoring.cm.local/ansible-vault.pass
```




## deployment

```
ansible-playbook --inventory inventories/icinga2 --vault-password-file vault/ansible-vault.pass playbooks/prepare.yml
ansible-playbook --inventory inventories/icinga2 --vault-password-file vault/ansible-vault.pass playbooks/master.yml
ansible-playbook --inventory inventories/icinga2 --vault-password-file vault/ansible-vault.pass playbooks/satellite.yml

```


## ansible

```
ansible -i hosts -m debug -a 'var=hostvars[inventory_hostname]' monitoring --vault-password-file host_vars/monitoring.cm.local/ansible-vault.pass

time ansible-playbook  --inventory=hosts monitoring.yml --vault-password-file=host_vars/monitoring.cm.local/ansible-vault.pass
```

## icinga Api

### master uptime
```
curl --user icinga2:S0mh1TuFJI --silent --insecure --header 'Accept: application/json' https://192.168.130.20:5665/v1/status/CIB | jq --raw-output '.results[].status.uptime'
```

### connected endpoints
```
curl --user icinga2:S0mh1TuFJI --silent --insecure --header 'Accept: application/json' https://192.168.130.20:5665/v1/status/ApiListener | jq --raw-output ".results[].status"
```
