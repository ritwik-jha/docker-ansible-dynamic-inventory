# Launching Docker Container and Dynamically Updating the Ansible Inventory for Further Configuration

## Project Description 

In this project we create a playbook which will install docker-ce software on the managed node and launch a docker container with port 80 exposed. We retrieve the ip of this docker container and update the inventory so that we can use ansible for further configuration of docker container.  
Here we will use localhost as managed node ie. we will be doing everything on our own system. 

### Description of Playbook
The ansible playbook will do the following tasks for us:
1. Configure yum repositiory 
2. Install docker-ce software
3. Start the docker services
4. Pull httpd image
5. Launch a docker container using httpd image (the port 8080 of base system routes to port 80 of container)
6. Retrieve the ip's of the launched docker container and update the inventory


## Steps

By default the containers have ssh disabled so we cannot use ansible for configuring the containers as asnible uses ansible and port 22 for contacting the container. 
So either we can use an image which has ssh enabled or after container launch we can manually enable ssh by installing the desired softwares. 

During the container launch we use register key to store the info about the container and then use blockinfile module of ansible to update the ansible inventory with the ip of the container. 

We run the **con_launch.yml** playbook to configure the localhost and launch the containers. 
This will configure docker repo, install docker software, launch docker container and update the ansible inventory.

Then we can use the **configure.yml** playbook to configure the container. (provided that the container has ssh enabled)

![con_launch.yml](images/t-14-2.png)

To run the playbook:

```yml
ansible-playbook con_launch.yml
```

**Results:**
![results](images/t-14-1.png)

So now the container has been launched and the inventory has been updated.

As we have used httpd image the webserver is running thus if we try to access the 8080 port of base system it would route us to the 80 port of the container where the webserver is running. 

![http_request](images/t-14-3.png)

### Ansible Inventory 

![inventory](images/t-14-4.png)

Now we can use configure.yml to configure the container (after enabling ssh).
To run configure.yml playbook:

```yml
ansible-playbook configure.yml
```
