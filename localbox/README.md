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


Until now, we add all the services necessary for running our application.
Let focus on how to deploy the project on the environment we just setup.

Encrypt ssh keys using ansible vault password:
First of all create a password file ~/vault_pass.txt and keep your password. PLEASE MAKE SURE YOU DO NOT UPLOAD THIS PASSWORD to repository.

Then encrypt your file secrets.yml file which contains ssh keys using following command:
$ansible-vault encrypt provision/deploy/vars/secrets.yml --vault-password-file ~/.vault_pass.txt
 This will encrypt content of the file and now it is safe to put on repository and no body can guess what is the actual contents are
 [You may need to install if it is already not installed yet]
 $sudo pip install cryptography
 If the above command does not work, you may also need to upgrade pip $pip install --upgrade pip
 
 Before provisioning you can decrypt using ansible vault
 $ansible-vault decrypt provision/deploy/vars/secrets.yml --vault-password-file ~/.vault_pass.txt

 Now replacing apache with Nginx and PHP-FPM
