## Local Development Setup for Chef

### Download and install

[chef-dk](http://www.getchef.com/downloads/chef-dk) - Does not currently support Windows or Mac OSX earlier than Mavericks

[virtual box](https://www.virtualbox.org/wiki/Downloads)

[vagrant](http://www.vagrantup.com/downloads.html)

[test-kitchen](http://kitchen.ci)

[berkshelf](http://berkshelf.com)

### If you are not able to run chef-dk check out this blog post by Mischa Taylor:
[Set Up a Sane Ruby Cookbook Authoring Environment for Chef on Mac OS X, Linux and Windows](http://misheska.com/blog/2013/12/26/set-up-a-sane-ruby-cookbook-authoring-environment-for-chef/)

### You may want to set the embedded ruby shipped with Chef first in your PATH
`echo 'export PATH="/opt/chefdk/embedded/bin:$PATH"' >> $HOME/.bash_profile`

### Make sure ruby and bundler are installed and working
`which ruby && ruby -v`

`which bundle && bundle -v`

### Install kitchen-vagrant gem if not already present
`chef gem install kitchen-vagrant`

## Local cookbook development workflow

### create new cookbook using Berkshelf
`berks cookbook pipeline`

### list test-kitchen VM's
`cd pipeline`

### add jenkins dependency to metadata.rb
`depends 'jenkins'`

### use Berkshelf to download dependency cookbooks
`berks install`

### Add jenkins to recipes/default.rb to create a jenkins master
### and also install java

`include_recipe 'jenkins::java'`

`include_recipe 'jenkins::master'`

### Modify .kitchen.yml to configure port forwarding, cpu/memory settings, update platform to centos-6.5, and set default recipe and attributes
```
driver:
  name: vagrant

driver_config:
  network:
  - ["forwarded_port", {guest: 8080, host: 8080}]
  customize:
    memory: 2048
    cpus: 2

provisioner:
  name: chef_solo

platforms:
  - name: centos-6.5

suites:
  - name: default
    run_list:  pipeline::default
    attributes:
      jenkins:
        master:
          install_method: package
```

### Show all kitchen instances
`kitchen list`

### Run VM for CentOS 6.5 and converge with configuration
`kitchen converge centos`

### See if jenkins is running
`http://localhost:8080`


### see git status of files added and modified
`git status`

### add all files
`git add .`

### commit all changes
`git commit -m "initial commit of pipeline cookbook"`


## Links to example cookbook
[Pipeline Cookbook Example](https://github.com/stephenlauck/setup_local_chef_dev_pipeline)
