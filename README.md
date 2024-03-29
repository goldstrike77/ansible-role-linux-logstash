![](https://img.shields.io/badge/Ansible-logstash-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments. The author does not guarantee the accuracy, completeness, reliability, and availability of the role content. Under no circumstances will the author be held responsible or liable in any way for any claims, damages, losses, expenses, costs or liabilities whatsoever, including, without limitation, any direct or indirect damages for loss of profits, business interruption or loss of information.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。作者不对角色内容之准确性、完整性、可靠性、可用性做保证。在任何情况下，作者均不对任何索赔，损害，损失，费用，成本或负债承担任何责任，包括但不限于因利润损失，业务中断或信息丢失而造成的任何直接或间接损害。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_logstash.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
  * [Architecture](#Architecture)
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
- [Donations](#Donations)

## Overview
Logstash is a free and open server-side data processing pipeline, Is the “L” in the ELK Stack - the world’s most popular log analysis platform and is responsible for aggregating data from different sources, processing it, and sending it down the pipeline, usually to be directly indexed in Elasticsearch. Logstash can pull from almost any data source using input plugins, apply a wide variety of data transformations and enhancements using filter plugins, and ship the data to a large number of destinations using output plugins. The role Logstash plays in the stack, therefore, is critical — it allows you to filter, massage, and shape your data so that it’s easier to work with.

### Architecture
<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/elk_arch.png" /></p>

## Requirements
### Operating systems
This Ansible role installs Logstash on Linux operating system, including establishing a filesystem structure and server configuration with some common operational features, Will works on the following operating systems:

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
* `logstash_path`: The directory that Logstash and its plugins use for any persistent needs.
* `logstash_rotate_day`: Specify the logs retention days.
* `logstash_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.

##### Role dependencies
* `logstash_elastic_stack_dept`: A boolean to determine whether or not use Elastic Stack components as same environment.

##### Elastic Stack parameters
* `logstash_elastic_stack_auth`: A boolean to determine whether or not enable authentication.
* `logstash_elastic_stack_https`: A boolean to determine whether or not Encrypting HTTP client communications.
* `logstash_elastic_stack_user`: Authorization user name, do not modify it.
* `logstash_elastic_stack_pass`: Authorization user password.
* `logstash_elastic_hosts`: List of Elasticsearch hosts Logstash should connect to.
* `logstash_elastic_port`: Elasticsearch REST port.
* `logstash_elastic_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.
* `logstash_elastic_memory_lock`: A boolean value, whether lock the process address space into memory on startup.
* `logstash_elastic_node_type`: Type of nodes`: default, master, data, ingest and coordinat.
* `logstash_kibana_port`: Kibana server port.
* `logstash_kibana_proxy`: A boolean to determine whether or no running behind a Proxy.
* `logstash_kibana_ngx_dept`: A boolean to determine whether or not proxy web interface and API traffic using NGinx.
* `logstash_kibana_ngx_domain`: Defines domain name.
* `logstash_kibana_ngx_port_http`: NGinx HTTP listen port.
* `logstash_kibana_ngx_port_https`: NGinx HTTPs listen port.

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
* `datacenter`: Define the DataCenter.
* `domain`: Define the Domain.
* `customer`: Define the customer name.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

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

    [siem:vars]
    logstash_cluster='siem'
    logstash_version='7.11.2'

    [siem]
    node01 ansible_host='192.168.1.10'
    node02 ansible_host='192.168.1.11'
    node03 ansible_host='192.168.1.12'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-logstash
       logstash_cluster: 'siem'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`.

```yaml
logstash_version: '7.11.2'
logstash_cluster: 'siem'
logstash_path: '/data'
logstash_rotate_day: '180'
logstash_heap_size: '2g'
logstash_elastic_stack_dept: true
logstash_elastic_stack_auth: true
logstash_elastic_stack_https: true
logstash_elastic_stack_user: 'elastic'
logstash_elastic_stack_pass: 'changeme'
logstash_elastic_hosts:
  - 'localhost'
logstash_elastic_port: '9200'
logstash_elastic_heap_size: '1g'
logstash_elastic_memory_lock: false
logstash_elastic_node_type: 'default'
logstash_kibana_port: '5601'
logstash_kibana_proxy: false
logstash_kibana_ngx_dept: true
logstash_kibana_ngx_domain: 'siem.example.com'
logstash_kibana_ngx_port_http: '80'
logstash_kibana_ngx_port_https: '443'
logstash_inputs_arg:
  common:
    - name: 'syslog-gelf'
      protocol: 'udp'
      port: '12201'
logstash_port_arg:
  exporter: '9198'
  api: '9600'
logstash_arg:
  index_refresh_interval: '30s'
  pipeline_workers: '16'
  pipeline_batch_size: '5000'
  pipeline_batch_delay: '100'
environments: 'prd'
datacenter: 'dc01'
domain: 'local'
customer: 'demo'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'China'
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

## Donations
Please donate to the following monero address.

    46CHVMbb6wQV2PJYEbahb353SYGqXhcdFQVEWdCnHb6JaR5125h3kNQ6bcqLye5G7UF7qz6xL9qHLDSAY3baagfmLZABz75