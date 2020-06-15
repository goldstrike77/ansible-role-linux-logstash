![](https://img.shields.io/badge/Ansible-logstash-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_logstash.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Logstash Versions](#logstash-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
Logstash is a free and open server-side data processing pipeline, Is the “L” in the ELK Stack - the world’s most popular log analysis platform and is responsible for aggregating data from different sources, processing it, and sending it down the pipeline, usually to be directly indexed in Elasticsearch. Logstash can pull from almost any data source using input plugins, apply a wide variety of data transformations and enhancements using filter plugins, and ship the data to a large number of destinations using output plugins. The role Logstash plays in the stack, therefore, is critical — it allows you to filter, massage, and shape your data so that it’s easier to work with. 

## Requirements
### Operating systems
This Ansible role installs Logstash on linux operating system, including establishing a filesystem structure and server configuration with some common operational features. This role will work on the following operating systems:

  * CentOS 7

### Logstash versions

The following list of supported the Logstash releases:

  * logstash 6.8+

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `logstash_version`: Specify the Logstash version.
* `logstash_cluster`: Specifies the name of the cluster.
* `logstash_path`: This directory is used to store server state.
* `logstash_rotate_day`: Specify the logs retention days.
* `logstash_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.

##### Role dependencies
* `logstash_elastic_stack_dept`: A boolean value, whether Elastic Stack components use the same environment.

##### Elastic Stack parameters
* `logstash_elastic_stack_auth`: A boolean value, Enable or Disable authentication.
* `logstash_elastic_stack_https`: A boolean value, whether Encrypting HTTP client communications.
* `logstash_elastic_stack_user`: Authorization user name, do not modify it.
* `logstash_elastic_stack_pass`: Authorization user password.
* `logstash_elastic_stack_version`: Specify the Elastic Stack version.
* `logstash_elastic_hosts`: List of Elasticsearch hosts Logstash should connect to.
* `logstash_elastic_port`: Elasticsearch REST port.
* `logstash_elastic_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.
* `logstash_elastic_memory_lock`: A boolean value, whether lock the process address space into memory on startup.
* `logstash_elastic_path`: Specify the ElasticSearch data directory.
* `logstash_elastic_node_type`: Type of nodes`: default, master, data, ingest and coordinat.
* `logstash_kibana_port`: Kibana server port.
* `logstash_kibana_proxy`: Whether running behind a HaProxy.
* `logstash_kibana_ngx_dept`: Whether proxy web interface and API traffic using NGinx.
* `logstash_kibana_ngx_domain`: Defines domain name.
* `logstash_kibana_ngx_port_http`: NGinx HTTP listen port.
* `logstash_kibana_ngx_port_https`: NGinx HTTPs listen port.
* `logstash_kibana_ngx_site_path`: Specify the NGinx site directory.
* `logstash_kibana_ngx_logs_path`: Specify the NGinx logs directory.

##### Inputs Variables
* `logstash_inputs_arg`: Define the global common inputs parameters.

##### Listen port
* `logstash_port_arg.exporter`: Port for prometheus exporter.
* `logstash_port_arg.api`: Port for the metrics REST endpoint.

##### Server System Variables
* `logstash_arg.index_refresh_interval`: How often to perform a index refresh operation.
* `logstash_arg.pipeline_workers`: The number of workers.
* `logstash_arg.pipeline_batch_size`: How many events to retrieve from inputs before sending to workers.
* `logstash_arg.pipeline_batch_delay`: How long to wait in milliseconds while polling for the next event.

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_http_port`: The consul HTTP API port.
* `consul_public_clients`: List of public consul clients.

### Other parameters
There are some variables in vars/main.yml:
* `logstash_kernel_parameters`: Operating system variables.

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)
- [Kibana](https://github.com/goldstrike77/ansible-role-linux-kibana.git) 
- [Elasticsearch](https://github.com/goldstrike77/ansible-role-linux-elasticsearch.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    [syslog:vars]
    logstash_cluster='syslog'
    logstash_version='7.6.2'

    [syslog]
    node01 ansible_host='192.168.1.10'
    node02 ansible_host='192.168.1.11'
    node03 ansible_host='192.168.1.12'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-logstash
       logstash_cluster='syslog'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

```yaml
logstash_version: '7.6.2'
logstash_cluster: 'syslog'
logstash_path: '/data'
logstash_rotate_day: '180'
logstash_heap_size: '2g'
logstash_elastic_stack_dept: false
logstash_elastic_stack_auth: false
logstash_elastic_stack_https: false
logstash_elastic_stack_user: 'elastic'
logstash_elastic_stack_pass: 'changeme'
logstash_elastic_stack_version: '{{ logstash_version }}'
logstash_elastic_hosts: 'localhost'
logstash_elastic_port: '9200'
logstash_elastic_heap_size: '1g'
logstash_elastic_memory_lock: false
logstash_elastic_path: '/data'
logstash_elastic_node_type: 'default'
logstash_kibana_port: '5601'
logstash_kibana_proxy: false
logstash_kibana_ngx_dept: false
logstash_kibana_ngx_domain: 'syslog.example.com'
logstash_kibana_ngx_port_http: '80'
logstash_kibana_ngx_port_https: '443'
logstash_kibana_ngx_site_path: '/data/nginx/site'
logstash_kibana_ngx_logs_path: '/data/nginx/logs'
logstash_inputs_arg:
  - name: 'cisco-asa'
    protocol: 'udp'
    port: '10514'
  - name: 'azure-nsg'
    sa_name: 'xxxxx'
    sa_access_key: 'xxxxx'
    sa_container: 'xxxxx'
logstash_port_arg:
  exporter: '9198'
  api: '9600'
logstash_arg:
  index_refresh_interval: '30s'
  pipeline_workers: '16'
  pipeline_batch_size: '5000'
  pipeline_batch_delay: '100'
environments: 'SIT'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'IDC01'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
