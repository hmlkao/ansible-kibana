# ansible-kibana
Ansible role for 5.x Elastic Kibana used as a part of ELK Stack.
Tested platforms are:
* CentOS 7

This manual is based on [official Elastic Ansible role for Elasticsearch](https://github.com/elastic/ansible-elasticsearch)

## Usage
Create your Ansible playbook with your own tasks and include role kibana. You will have to have this repository accessible within the context of playbook, e.g.

```
cd /my/repos/
git clone https://github.com/hmlkao/ansible-kibana.git
cd /my/ansible/playbook
mkdir -p roles
ln -s /my/repos/ansible-kibana ./roles/kibana
```

Then create your playbook yaml adding the role kibana.
By default, the user is only required to specify .

The simplest configuration therefore consists of:
```
- name: Simple Example
  hosts: localhost
  roles:
    - { role: kibana,
      kibana_config: {
        server.host: 0.0.0.0,
      }
    }
```

The above runs Kibana on localhost with default values.

Extended configuration with X-pack enabled:
```
- name: Extended Example
  hosts: localhost
  roles:
    - { role: kibana,
      kibana_config: {
        server.host: 0.0.0.0,
      }
    }
  vars:
    kibana_enable_xpack: true
```

## Role variables
```
kibana_config               ([]) Variables from [Kibana onfiguration](https://www.elastic.co/guide/en/kibana/current/settings.html)
kibana_version              (5.6.5) Version of Kibana which will be installed
kibana_enable_xpack         (false) Enable X-pack features in Kibana
kibana_xpack_custom_url     URL for X-pack download
kibana_log_to_file          (false) Kibana defaultly logs to journal, if set stdout will be redirected to file
kibana_plugins_reinstall    (false) Force reinstall all plugins
kibana_plugins              (false) An array of plugin definitions
```
