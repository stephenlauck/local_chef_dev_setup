## Download and install

[chef-dk](http://www.getchef.com/downloads/chef-dk)
Does not currently support Windows or Mac OSX earlier than Mavericks

[virtual box](https://www.virtualbox.org/wiki/Downloads)

[vagrant](http://www.vagrantup.com/downloads.html)

### If you run into problems running chef-dk, maybe check out this blog:
[Set Up a Sane Ruby Cookbook Authoring Environment for Chef on Mac OS X, Linux and Windows](http://misheska.com/blog/2013/12/26/set-up-a-sane-ruby-cookbook-authoring-environment-for-chef/)

### You may want to set the embedded ruby shipped with Chef first in your PATH
`echo 'export PATH="/opt/chefdk/embedded/bin:$PATH"' >> $HOME/.bash_profile`

### Make sure ruby and bundler are installed and working
`$ which ruby && ruby -v`
`$ which bundle && bundle -v`

### Install kitchen-vagrant gem if not already present
`$ chef gem install kitchen-vagrant`

## Local cookbook development workflow

### Clone community python cookbook
`$ git clone git@github.com:poise/python.git`

### list test-kitchen VM's
`$ cd python`

`$ kitchen list`

### add .kitchen.local.yml file with a platform for CentOS 6.5
```
platforms:
- name: centos-6.5
  driver_config:
    box: opscode-centos-6.5
    box_url: https://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box
```

### Run VM for CentOS 6.5 and install python 2.7 from source
`$ kitchen converge source-centos-65`


### Log into converged VM and check python version
`$ kitchen login source-centos-65`

`[vagrant@source-centos-65 ~]$ python -v`
