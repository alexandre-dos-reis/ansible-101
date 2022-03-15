## Provision a server with Vagrant and Ansible

### Vagrant
- You can use vagrant to run ansible playbook to provision a server !
- Have Vagrant installed locally
- Create a Vagrantfile by typing `vagrant init`

- Fill the file with :

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
  end
end
```

### Ansible

- Create a file named `playbook.yml` that will be run by vagrant.
- Describe your steps :

```yml
---
- hosts: all
  become: true
  tasks:

    - name: Ensure NTP is installed.
      apt: name=ntp state=present
    
    - name: Ensure NTP is running.
      service: name=ntp state=started enable=yes
```

### Provision

- Run `vagrant up` if the server isn't created yet.
- Run `vagrant provision` if server is already running.

- Everytime you make a change to your `playbook.yml`, you can run the playbook with `vagrant provision`.

## Idempotence

...