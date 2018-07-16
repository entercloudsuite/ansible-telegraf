Ansible Role: telegraf 
======================================

[![Build Status](https://travis-ci.org/entercloudsuite/ansible-telegraf.svg?branch=master)](https://travis-ci.org/entercloudsuite/ansible-telegraf)
[![Galaxy](https://img.shields.io/badge/galaxy-entercloudsuite.telegraf-blue.svg?style=flat-square)](https://galaxy.ansible.com/entercloudsuite/telegraf)  

Installs telegraf on Ubuntu 16.04 (Xenial)

## Requirements

This role requires Ansible 2.4 or higher.
This role is a copy of dj-wasabi.telegraf

## Role Variables
Check the role doc
https://github.com/dj-wasabi/ansible-telegraf/blob/master/README.md

## Example Playbook
``` yaml
Run with default vars:

    - hosts: all
      roles:
        - { role: ansible-telegraf }
```
# Example.2 Playbook

```yaml
---
- name: run the main role
  hosts: all
  become_method: su
  roles:
    - role: dj-wasabi.telegraf
      telegraf_global_tags:
        - tag_name: web
          tag_value: web_server
        - tag_name: environment
          tag_value: production
      telegraf_agent_output:
        - type: influxdb
          config:
            - urls = ["http://localhost:8086"]
            - timeout = "5s"
            - database = "mydatabases"
            - username = "myusername"
            - password = "verySecret"
      telegraf_plugins_default:
        - plugin: cpu
          config:
            - percpu = true
            - totalcpu = true
            - collect_cpu_time = false
        - plugin: disk
          config:
            - ignore_fs = ["tmpfs", "devtmpfs"]
        - plugin: io
        - plugin: mem
        - plugin: net
        - plugin: system
        - plugin: swap
        - plugin: netstat
        - plugin: processes
        - plugin: kernel
        - plugin: sysstat
          config:
            - interval = "60s"
            - sadc_path = "/usr/lib/sysstat/sadc"
      telegraf_plugins_extra:
        mysql:
          config:
            - servers = ["root:v3ryS3cr3t@tcp(localhost:3306)/"]
        memcached:
          config:
            - servers = ["localhost:11211"]
        postgresql:
          config:
            - address = "postgres://telegraf:verysecret_Password@localhost/db"
```
## Testing

Tests are performed using [Molecule](http://molecule.readthedocs.org/en/latest/).

Install Molecule or use `docker-compose run --rm molecule` to run a local Docker container, based on the [enterclousuite/molecule](https://hub.docker.com/r/fminzoni/molecule/) project, from where you can use `molecule`.

1. Run `molecule create` to start the target Docker container on your local engine.  
2. Use `molecule login` to log in to the running container.  
3. Edit the role files.  
4. Add other required roles (external) in the molecule/default/requirements.yml file.  
5. Edit the molecule/default/playbook.yml.  
6. Define infra tests under the molecule/default/tests folder using the goos verifier.  
7. When ready, use `molecule converge` to run the Ansible Playbook and `molecule verify` to execute the test suite.  
Note that the converge process starts performing a syntax check of the role.  
Destroy the Docker container with the command `molecule destroy`.   

To run all the steps with just one command, run `molecule test`. 

In order to run the role targeting a VM, use the playbook_deploy.yml file for example with the following command: `ansible-playbook ansible-telegraf/molecule/default/playbook_deploy.yml -i VM_IP_OR_FQDN, -u ubuntu --private-key private.pem`.  

## License

MIT
