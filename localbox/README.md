### add initial file structure for setting up service and dependecies application needs
```
$mkdir -p provision/{setup,deploy}/{tasks,vars,handlers}
```
### Add file structure for deploying the application in the box after all required service installed
```
$touch provision/{setup,deploy}/{tasks,vars,handlers}/main.yml
```
Folder structure should be looking like following
```
setup/
    tasks/
        main.yml
    vars/
        main.yml
    handlers/
        main.yml
deploy/
    tasks/
        main.yml
    vars/
        main.yml
    handlers/
        main.yml
vagrant.yml
Vagrantfile
.gitignore
```
```
$touch provision/vagrant.yml
```

###Now add following in Vagrantfile to tell we will use ansible to provision
```
# Install required software, dependencies and configurations on the
# virtual machine using a provisioner.
config.vm.provision "ansible" do |ansible|
  ansible.playbook = "provision/vagrant.yml"
end
``` 

###Then add following to provision/vagrant.yml:

```
- hosts: all
  roles:
      - setup
      - deploy
```