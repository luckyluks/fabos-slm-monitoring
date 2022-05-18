Ansible Role: monitoring
=========

The role creates / destroys a monitoring stack. It is designed to be used on a FabOS SLM resource.

Requirements
------------

The requirments of ["community.docker"](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html) collection are installed during setup.

Role Variables
--------------

The role requires three variables to be set correctly:

| **variable**            | **type** | **default** | **description**                                                             |
|-------------------------|----------|-------------|-----------------------------------------------------------------------------|
| **host_registry_fqdn**  | string   | -   | the **full** qualified host name of the registry host                       |
| **host_localhost_fqdn** | string   | -   | the **full** qualified hostname of the local host                           |
| **asset_id**            | string   | -   | arbitrary ID (e.g. UUID) which is used to identify the host in the registry |


The variables can be set either via the "vars" keyword or in the `vars/main.yaml` file. 

Other variables are set by default in: `defaults/main.yaml`:

    # default compose settings
    docker_compose_target_folder: "/tmp/monitoring"

    # default service ports
    docker_port_prometheus: 9090
    docker_port_nodeexporter: 9100
    docker_port_cadvisor: 8080
    docker_port_aasrouter: 4001

    # default docker images
    docker_image_prometheus: "prom/prometheus:v2.34.0"
    docker_image_nodeexporter: "prom/node-exporter:v1.3.1"
    docker_image_cadvisor: "gcr.io/cadvisor/cadvisor:v0.38.6"
    docker_image_aasrouter: "fabos4ai/aas-router:1.0.0-snapshot"

Dependencies
------------

Collection ["community.docker"](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html) is used to communicate with docker.

Example Playbook
----------------

The monitoring stack can be started with:

    - hosts: localhost
      connection: local
      tasks:
        - include_role:
            name: fabos-slm-monitoring
            tasks_from: create.yml
          vars:
            - host_registry_fqdn: "registry.example.com"
            - host_localhost_fqdn: "server.example.com"
            - asset_id: "defaultID" 

The monitoring stack can be stopped/teared down with:

    - hosts: localhost
      connection: local
      tasks:
        - include_role:
            name: fabos-slm-monitoring
            tasks_from: destroy.yml

License
-------

MIT

Author Information
------------------

Lukas Rauh - Fraunhofer Institute for Manufacturing Engineering and Automation IPA
