Ansible docker swarm roles
==========================

Roles to spin up a small docker swarm quickly. Tested on ubuntu server 22.04.

Setup
------

### .env

Create a .env file in the repository folder. Update the env variable values as needed.

```
SSH_KEY=/home/user/.ssh/id_rsa
DOCKER_CERTS=/home/user/folder
```

Create a hosts file in the repository folder. Update the variable values as needed.

### hosts

```yml
all:

  hosts:

    worker:
      ansible_host: xx.xxx.xxx.xxx
      ansible_connection: ssh
      ansible_user: root
      ansible_ssh_extra_args: '-o StrictHostKeyChecking=no'
    manager:
      ansible_host: xx.xxx.xxx.xxx
      ansible_connection: ssh
      ansible_user: root
      ansible_ssh_extra_args: '-o StrictHostKeyChecking=no'

  children:

    swarm_managers:
      hosts:
        manager:
      vars:
        docker_cert_passphrase: passhere
        docker_data_root: /home/docker
        client_cert_folder_path: /home/user/.docker/manager_certs
    swarm_workers:
      hosts:
        worker:
      vars:
        swarm_manager_addr: manager.domain.net:2377
    backup_servers:
      hosts:
        worker:
```

### Run the play

```bash
ansible-playbook /var/www/ansible/main.yml
# or
docker compose run play
```

### Docker daemon manager access

You can remotely connect to the manager:

```bash
export DOCKER_HOST=tcp://manager.domain.net:4444 DOCKER_TLS_VERIFY=1 DOCKER_CERT_PATH=/home/user/.docker/manager_certs
```

Time to deploy some stacks now.
