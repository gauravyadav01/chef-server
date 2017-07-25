# Chef-server

This repository to help you configure your development environment with Chef-server. You can configure chef-server as in memory chef-zero 
or full GUI support. 
This repository is using batali to solve cookbook dependency. 
It is using vagrant landrush plugin for DNS purpose. 

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. 
Please add running user to sudoers file to configure dnsmasq and dns otherwise it will ask sudo permisson to configure dns.

install Chef-dk from Chef website
https://downloads.chef.io/chefdk

Follow below instructions
```
git clone https://github.com/gauravyadav01/chef-server.git
cd chef-server
gem install
batali update
vagrant up chef-server
```

If will have in memory chef-server 
Run below command to upload all cookbook, roles, environment and data_bags to chef-server

```
knife upload .
```

you can run vagrant up chef-node1 further to spin up instance.


## Advance Configuration
You can modify instance variable from servers.yaml. 

Example of servers.yml file

```
---
  :domain: example.com
  :network: 192.168.10.0/24
  :box: bento/centos-7.3
  :vms:
    - :name: chef-server
      :ip: 192.168.10.2
      :role: my_chef_zero
      :cpu: 2
      :memory: 2048
    - :name: chef-node1
      :ip: 192.168.10.3
      :role: base
```

First level of hierarchy represents global variable
 1. domain
 2. network
 3. box 
 
Second level of hierarchy represnts instance level variable( vms)
 1. name
 2. role
 3. cpu 
 4. memory
 
 if you won't define cpu and memory, it will be 1 core and 1048 mb memory.
 
 ## How to use batali to resolve dependecy
 write all wrapper cookbook in site-cookbooks. Include all dependency in metadata.rb for cookbook.
 Write a Batali file as below to resolve dependency. Make sure it start with capital B. Default source will super market. 
 Wrapper cookbook path will you site-cookbooks directory.
 
 ```
 Batali.define do
  source 'https://supermarket.chef.io'
  cookbook 'my_chef_zero', path: 'site-cookbooks/my_chef_zero'
  cookbook 'my_chef_server', path: 'site-cookbooks/my-chef-server'
  cookbook 'base', path: 'site-cookbooks/base'
end
```
 
