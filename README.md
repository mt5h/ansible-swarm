# Simple Ansible and Docker Swarm orchestration

This role show how is possibile to orchestrate a swarm application with Ansible with minimal docker stack changes. No templates involved.

## Minimal requirements
- A Docker Swarm cluster
- Install on the docker host python docker
 ```
 pip install docker
 ```
- All stack files must be present under the files/{{ env }}/swarm\_apps/{{ your swarm stack name }}. You can follow the structure in the files folder.
- Adjust the inventory config to point to your host.

The base idea is that configs, secrets and networks are all external, no need to copy all the files to the remote host. Only the stack is being copied to the swarm node.

Basic config of your stack is:

```
env: stage
swarm_folder: /opt/swarm_stacks
swarm_apps:
- name: mystack
  stack_file: stack.yml
  configs:
    - name: mysql_config
      file: mysql_config.cnf
  secrets:
    - name: mysql_password
      content: "password123"
  networks:
    - name: mysql_network
```

Configs are read as files
Secrets are read as string (use Ansible Vault for a better security)

Use the _env_ variable to use different files for different environment.

The main catch from this role is to roll secrets and configurations automatically whitout persisiting files and automatically reload the container in the stack when changes happen.

## How to run the playbook
```
ansible-playbook -i inventory.yml play.yml
```


