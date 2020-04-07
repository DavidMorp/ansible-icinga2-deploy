# Ansible deployment of a icinga2 cluster

Demonstrator um verschiedene Szenarien zu testen und die Entwicklung der ansible-icinga2 Rolle zu testen.

## usage

download external ansible roles

```
$ ansible-galaxy install -r requirements.yml [--force]
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

time ansible-playbook --inventory hosts --vault-password-file host_vars/monitoring.cm.local/ansible-vault.pass monitoring.yml

...

real    4m13,517s
user    1m46,355s
sys     0m14,266s
```


## ansible

```
ansible -i hosts -m debug -a 'var=hostvars[inventory_hostname]' monitoring --vault-password-file host_vars/monitoring.cm.local/ansible-vault.pass

time ansible-playbook  --inventory=hosts monitoring.yml --vault-password-file=host_vars/monitoring.cm.local/ansible-vault.pass
```
