# ansible-bootstrap-test-setup
In this project I create a test setup with a single docker container. 
The container will be bootstrapped with a special user for running ansible playbooks. 

Under the directory vagrant-setup I will experiment with a setup connecting to multiple virtual servers

The root password in the docker container should be blank in the default docker image.
So before doing anything else I could log in and change it before running the ansible bootstrap script.

Command for building the docker image:
docker build --tag "ubuntu-test-server" --file "./Dockerfile" .

Command for starting the container:
docker run -it -p 127.0.0.1:2222:22 --name "default-ansible-host" "ubuntu-test-server"
or run with -d and user docker start ID afterwards

Going into the server and setting the root password:
docker exec -it containerID bash
changed the root password to "admin" with "passwd"
Start the ssh deamon:
/usr/sbin/sshd  (TODO: put into Dockerfile)

Add a user, e.g. nikko:
useradd -m nikko
set password for nikko:
passwd nikko
log ind with ssh with the password
copy and install an ssh key for the user to the server:
ssh-copy-id -i ~/.ssh/id_ed25519.pub 192.168.56.2
this copies the key to the users ~/.ssh/authorized_keys file (creates the dir if not exists)
this can also be done with scp

give a user sudo rights:
usermod -aG sudo username
test it out with: sudo whoami

Ad hoc test command:
ansible all --key-file ~/.ssh/ansible -i inventory -m ping
And with the cfg and inventory file
ansible all -m ping

Command for running the bootstrap ansible script:
ansible-playbook --ask-become-pass bootstrap.yml

TODO: jeg har lavet SSH til docker containeren med simones password, hvordan får jeg det sat op med en key file, se videor.
 - basic test setup med docker containeren
 - brug vagrant til at lave et setup som Jay; flere servere, brug template files som i eksemplet
   - brug evt tmux
 - kig på openstack



Vagrant setup:

vagrant up        (starts the vms)
vagrant ssh web   (ssh into vm and run: sudo useradd -m nikko -> then run: sudo passwd nikko )
now you can run ssh ip_of_vm
copy ansible key to vms:
ssh-copy-id -i ~/.ssh/ansible.pub 192.168.56.3
Now you can ssh to the vms with:
ssh -i ~/.ssh/ansible 192.168.56.3
