# ansible-elasticsearch
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-elastic.elasticsearch-blue.svg)](https://galaxy.ansible.com/elastic/elasticsearch/)

**THIS ROLE IS UNDER ACTIVE DEVELOPMENT AND IS NOT SUITABLE FOR USAGE IN PRODUCTION!!! .**

**THIS ROLE IS FOR 6.x, 5.x. FOR 2.x SUPPORT PLEASE USE THE 2.x BRANCH.**

Ansible role for 6.x/5.x Logstash.  Currently this works on Debian and RedHat based linux systems.  Tested platforms are:

* Debian 9

# Sample configuration
## Prereqs
TLS certs must be present at: /tmp/as_ansible/certs/ls-<ls_es_ssl_config['dns']>.p12 **<< variable used in sample config is: elasticsearch_dns**

## hosts.yml
```
logstash:
      hosts:
        <host>:
          allowed_pipelines: '["pipeline1", "pipeline2"]'
      vars:
        ls_version_lock: true
        ls_upgrade: false
        ls_version: "1:7.6.1-1"
```
## playbook.yml
```
- hosts: logstash
  any_errors_fatal: true
  vars:
    ls_conf_dir: /etc/logstash
  roles:
  - role: logstash
    ls_instance_name: "ls1"
    ls_memcached_install: <true/false>
    action_auto_create_index: true
    ls_config:
      node.name: "<inventory_hostname>"
      path.data: /var/lib/logstash
      path.logs: /var/log/logstash
      config.support_escapes: <true/false>
    ls_xpack_config:
      ls_monitoring_password: "logstash_user_password>"
      poll_interval: 5s
      allowed_pipelines: "{{ allowed_pipelines }}"
    ls_es_ssl_config:
      #If enabled https should be used to connect to elasticsearch
      dns: "<elasticsearch_dns>"
      elasticsearch.url: "https://<elasticsearch_hostnname>:<elasticsearch_port>"
      elasticsearch.username: "<logstash_user>"
      elasticsearch.password: "<logstash_user_password>"
    ls_enable_xpack: true
```