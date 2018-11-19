
Use the vagrant file provided to provision all nodes.
Vagrantfile given will spawn 3 application nodes(centos 6) and 1 ansible toolbox to run the ansible script.

Steps to Deploy:-

1. `vagrant up`
2. `vagrant ssh ansible-toolbox`
3. `cd /vagrant/ansible/`
4. Run <br>
   a. `ansible-playbook  -i hosts setup-haproxy.yml` 
   
   b. `ansible-playbook  -i hosts setup-app.yml`
   
TEST:

From inside Ansible-Toolbox Host (inside vagrant network bridge)
1. `curl http://192.168.30.1`

From Outside Vagrant Network:
1. `curl http://192.168.30.1:9081`
   
 
