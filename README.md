# ansible-kibana
Ansible role for 5.x Elastic Kibana used as a part of ELK Stack.
Tested platforms are:
* CentOS 7
Ansible version >= 2.4 must be used.

This manual is based on [official Elastic Ansible role for Elasticsearch](https://github.com/elastic/ansible-elasticsearch)

## Usage
Create your Ansible playbook with your own tasks and include role kibana. You will have to have this repository accessible within the context of playbook, e.g.

```bash
cd /my/repos/
git clone https://github.com/hmlkao/ansible-kibana.git
cd /my/ansible/playbook
mkdir -p roles
ln -s /my/repos/ansible-kibana ./roles/kibana
```

Then create your playbook yaml adding the role kibana.

By default, the user is only required to specify variable **kibana_instance_name** which is used to exactly identify an instance (different paths, configurations, etc. for different instances).

In the most cases there will be only one instance, eg. "node1"

### Simplest configuration
```yaml
- name: Simple Example
  hosts: localhost
  tasks:
    - import_role:
        name: kibana
      vars:
        kibana_instance_name: "node1"
```

The above runs Kibana on localhost with default values.

> Note: This configuration runs instance listening only on localhost, so it will be accessible only from localhost.

### Extended configuration with X-pack enabled
```yaml
- name: Extended Example
  hosts: some-hosts
  tasks:
    - import_role:
        name: kibana
      vars:
        kibana_instance_name: "node1"
        kibana_config: {
          server.host: 0.0.0.0,
        }
        kibana_enable_xpack: true
      tags:
        - kibana
```

The above runs Kibana on host(s) in some-hosts group.

Kibana will be listening on all interfaces on default port 5601.

Features of x-pack plugin will be enabled too.

### Configuration for multiple nodes on host(s)
```yaml
- name: Multiple Example
  hosts:
    - kibana-hosts
  tasks:
    - import_role:
        name: kibana
      vars:
        kibana_instance_name: "testing"
        kibana_config: {
          server.host: 192.168.0.200,
          server.port: 5602,
        }
        kibana_enable_xpack: true
    - import_role:
        name: kibana
      vars:
        kibana_instance_name: "production"
        kibana_config: {
          server.host: 0.0.0.0,
          elasticsearch.url: http://elasticsearch.example.com:9200,
          elasticsearch.username: elastic,
          elasticsearch.password: changeme,
        }
        kibana_enable_xpack: true
```

This configuration creates two instances of Kibana on all host(s) in kibana-hosts group

First instance named "testing" will be connected to elasticsearch on localhost:9200 and listening on interface 192.168.0.200 on port 5602.

Second instance named "production" will be connected to elasticsearch on host elasticsearch.example.com:9200 and listening on all interfaces on default port 5601.

Both will have enabled features of x-pack plugin.

## Role variables
`variable` (default value) Description

`kibana_config` ([]) Variables from [Kibana configuration](https://www.elastic.co/guide/en/kibana/current/settings.html)

`kibana_version` (5.6.5) Version of Kibana which will be installed

`kibana_enable_xpack` (false) Enable X-pack features in Kibana

`kibana_xpack_custom_url` URL for X-pack download

`kibana_log_to_file` (false) Kibana defaultly logs to journal, if set stdout will be redirected to file

`kibana_plugins_reinstall` (false) Force reinstall all plugins

`kibana_plugins` (false) An array of plugin definitions
