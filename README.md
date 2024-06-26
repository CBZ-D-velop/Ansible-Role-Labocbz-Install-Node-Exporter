# Ansible role: labocbz.install_node_exporter

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Prometheus](https://img.shields.io/badge/Tech-Prometheus-orange)
![Tag: Node_Exporter](https://img.shields.io/badge/Tech-Node_Exporter-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)

An Ansible role to install and configure Node_Exporter on your host.

The provided Ansible role automates the setup of Prometheus' Node Exporter, a vital tool for collecting essential system and hardware metrics crucial for comprehensive monitoring. By simplifying the intricate process of deploying Node Exporter instances, the role offers versatile customization options tailored to unique monitoring needs. It facilitates seamless installation and configuration while ensuring secure communication through SSL/TLS encryption.

Additionally, the role enables effortless metrics selection, empowering users to adapt Node Exporter to specific monitoring requirements. With provisions for fortified access control through basic authentication, the role contributes to heightened security. Ultimately, this Ansible role streamlines Node Exporter deployment, fostering efficient performance monitoring, proactive maintenance, and well-informed decision-making through its customization capabilities, security enhancements, and tailored metrics collection.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---

install_node_exporter__install_path: "/etc/node_exporter"
install_node_exporter__lib_path: "/usr/local/node_exporter"
install_node_exporter__web_ssl_path:  "{{ install_node_exporter__install_path }}/ssl"
install_node_exporter__web_config_file: "{{ install_node_exporter__install_path }}/web-config.yml"
install_node_exporter__loglevel: "debug"
install_node_exporter__log_path: "/var/log/node_exporter"

install_node_exporter__version: "1.6.1"
install_node_exporter__architecture: "amd64"

install_node_exporter__user: "root"
install_node_exporter__group: "root"

install_node_exporter__ssl: true
install_node_exporter__ssl_key: "{{ install_node_exporter__web_ssl_path }}/my-node_exporter-cluster.domain.tld/my-node_exporter-cluster.domain.tld.pem.key"
install_node_exporter__ssl_crt: "{{ install_node_exporter__web_ssl_path }}/my-node_exporter-cluster.domain.tld/my-node_exporter-cluster.domain.tld.pem.crt"

install_node_exporter__port: 9100
install_node_exporter__stats_exports:
  - "cpu" #Exposes CPU statistics
  - "cpufreq" #Exposes CPU frequency statistics
  - "diskstats" #Exposes disk I/O statistics.
  - "filesystem" #Exposes filesystem statistics, such as disk space used.
  - "loadavg" #Exposes load average.
  - "meminfo" #Exposes memory statistics.
  - "netdev" #Exposes network interface statistics such as bytes transferred.
  - "processes" #Exposes aggregate process statistics from /proc.
  - "netstat" #Exposes network statistics from /proc/net/netstat. This is the same information as netstat -s.
  - "mountstats" #Exposes filesystem statistics from /proc/self/mountstats. Exposes detailed NFS client statistics.
  - "systemd" #Exposes service and system status from systemd.
  - "uname"
  - "time"

install_node_exporter__basic_auth: true
install_node_exporter__basic_auth_login: "admin"
install_node_exporter__basic_auth_password_hash: "$2a$10$0M5Kx/KYWNIExB1AfP0wDuMT6hGkkNOcxLtLRWV6nfSZWfonGb69W"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---

inv_install_node_exporter__install_path: "/etc/node_exporter"
inv_install_node_exporter__lib_path: "/usr/local/node_exporter"
inv_install_node_exporter__web_ssl_path:  "{{ inv_install_node_exporter__install_path }}/ssl"
inv_install_node_exporter__web_config_file: "{{ inv_install_node_exporter__install_path }}/web-config.yml"
inv_install_node_exporter__loglevel: "debug"
inv_install_node_exporter__log_path: "/var/log/node_exporter"

inv_install_node_exporter__version: "1.6.1"
inv_install_node_exporter__architecture: "amd64"

inv_install_node_exporter__user: "root"
inv_install_node_exporter__group: "root"

inv_install_node_exporter__ssl: true
inv_install_node_exporter__ssl_key: "{{ inv_install_node_exporter__web_ssl_path }}/my-node_exporter-cluster.domain.tld/my-node_exporter-cluster.domain.tld.pem.key"
inv_install_node_exporter__ssl_crt: "{{ inv_install_node_exporter__web_ssl_path }}/my-node_exporter-cluster.domain.tld/my-node_exporter-cluster.domain.tld.pem.crt"

inv_install_node_exporter__port: 9100
inv_install_node_exporter__stats_exports:
  - "cpu" #Exposes CPU statistics
  - "cpufreq" #Exposes CPU frequency statistics
  - "diskstats" #Exposes disk I/O statistics.
  - "filesystem" #Exposes filesystem statistics, such as disk space used.
  - "loadavg" #Exposes load average.
  - "meminfo" #Exposes memory statistics.
  - "netdev" #Exposes network interface statistics such as bytes transferred.
  - "processes" #Exposes aggregate process statistics from /proc.
  - "netstat" #Exposes network statistics from /proc/net/netstat. This is the same information as netstat -s.
  - "mountstats" #Exposes filesystem statistics from /proc/self/mountstats. Exposes detailed NFS client statistics.
  - "systemd" #Exposes service and system status from systemd.
  - "uname"
  - "time"

inv_install_node_exporter__basic_auth: true
inv_install_node_exporter__basic_auth_login: "admin"
inv_install_node_exporter__basic_auth_password_hash: "$2a$10$0M5Kx/KYWNIExB1AfP0wDuMT6hGkkNOcxLtLRWV6nfSZWfonGb69W"

inv_install_node_exporter__basic_auth_password: "admin"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_node_exporter"
  tags:
    - "labocbz.install_node_exporter"
  vars:
    install_node_exporter__install_path: "{{ inv_install_node_exporter__install_path }}"
    install_node_exporter__lib_path: "{{ inv_install_node_exporter__lib_path }}"
    install_node_exporter__web_ssl_path: "{{ inv_install_node_exporter__web_ssl_path }}"
    install_node_exporter__web_config_file: "{{ inv_install_node_exporter__web_config_file }}"
    install_node_exporter__loglevel: "{{ inv_install_node_exporter__loglevel }}"
    install_node_exporter__log_path: "{{ inv_install_node_exporter__log_path }}"
    install_node_exporter__version: "{{ inv_install_node_exporter__version }}"
    install_node_exporter__architecture: "{{ inv_install_node_exporter__architecture }}"
    install_node_exporter__user: "{{ inv_install_node_exporter__user }}"
    install_node_exporter__group: "{{ inv_install_node_exporter__group }}"
    install_node_exporter__ssl: "{{ inv_install_node_exporter__ssl }}"
    install_node_exporter__ssl_key: "{{ inv_install_node_exporter__ssl_key }}"
    install_node_exporter__ssl_crt: "{{ inv_install_node_exporter__ssl_crt }}"
    install_node_exporter__port: "{{ inv_install_node_exporter__port }}"
    install_node_exporter__stats_exports: "{{ inv_install_node_exporter__stats_exports }}"
    install_node_exporter__basic_auth: "{{ inv_install_node_exporter__basic_auth }}"
    install_node_exporter__basic_auth_login: "{{ inv_install_node_exporter__basic_auth_login }}"
    install_node_exporter__basic_auth_password_hash: "{{ inv_install_node_exporter__basic_auth_password_hash }}"
  ansible.builtin.include_role:
    name: "labocbz.install_node_exporter"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-07: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-08-07-b: Confs, SSL/TLS, Auth

* Role can handle SSL/TLS
* Role handle loglevel and port configuration
* Crypto material have to be created and deployed before (no generation)
* Role doesn't handle the hashing of the password

### 2023-08-10: Log redirection

* Exporter lor are now redirected into a file

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-14: System users

* Role can now use system users and address groups

### 2024-02-22: New CICD and fixes

* Added support for Ubuntu 22
* Added support for Debian 11/22
* Edited vars for linting (role name and __)
* Added generic support for Docker dind (can add used for obscures reasons ... user in use)
* Fix idempotency
* Added informations for UID and GID for user/groups
* Added support for user password creation (on_create)
* New CI, need work on tag and releases
* CI use now Sonarqube

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
