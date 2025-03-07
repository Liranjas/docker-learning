# Docker Testing on WSL 2 (Debian)

Basic report on Docker Engine. Tried to use script utility but didn't like the result, so i've just come up with a plain README.

# Installing docker 

### Script that helps install the best Docker version for your environment
curl -fsSL https://get.docker.com | bash

### Set Docker daemon in rootless mode
dockerd-rootless-setuptool.sh install  ### Corrected command

### May need this for setting up too
sudo apt-get install -y uidmap

### Displays running containers and info
docker container ls
### OR
docker ps

### If everything is right, test fetching from the hub with a hello-world image
docker container run hello-world

# Basic Commands 

### Displays all containers on the host
docker container ls -a
### OR
docker ps -a

### Fetch and run Ubuntu image with an interactive terminal
docker container run -it ubuntu

### Do this inside the container
uptime
### NOTE: Pressing Ctrl+D will stop the container since the main process is /bin/bash.
### Instead, press Ctrl+P+Q to detach without stopping.

### Enter the container again
docker container attach <your_ubuntu_container_ID>

### Displays running processes inside the container
ps -ef

### Run an image with a specific name
docker container run --name your_container ubuntu

### Alternatively, create a container without starting it
docker container create --name your_container <image>

### Start an existing container
docker container start your_container

### Stop a running container
docker container stop <container_name>

## LAB:
### > Attach to a Ubuntu container and run `watch date`, it will display the current time (and date) every 2 sec.
### > Open another terminal and try to control the container with `docker container pause` and `docker container unpause`.
### > Alternatively, if you're using WSL and encounter difficulties, try just doing start/stop commands.
### This happens because WSL includes a translation layer between Linux and Windows resource management systems. WSL2 does not fully implement all Linux kernel features, particularly certain cgroup controllers like the freezer subsystem needed for container pausing.

### Remove a stopped container
docker container rm <container_name>

### Display real-time container resource usage statistics
docker container stats

### Show detailed information about a container or image
docker inspect <target>

### Show logs from a container
docker container logs <container_name>

### Fetch a specific image to your host without running it
docker pull centos:7

### Display all images
docker image ls -a

# Container management & networking 

### Run container in detached mode with -d flag (parameter)
docker container run -d --name my_nginx nginx

### When starting this container, there's no bash because the main process in the Nginx image isn't /bin/bash
docker ps

### Execute commands in a running container without attaching; "-ti" provides an interactive terminal interface
docker container exec -ti my_nginx ls  

### Start a bash shell inside the running container
docker container exec -ti my_nginx bash  

### Running this command outside the Nginx container to see related processes on the host
ps aux | grep nginx

### Split terminal view example with two previous commands:

![docker](https://github.com/user-attachments/assets/126f5bb0-7e73-45d9-aaa5-76e6c6e74977)


### Map your host's port 8080 to the Nginx container port 80
docker container run -d -p 8080:80 --name mapped_nginx nginx

### Now you can access http://localhost:8080 in your browser to see the Nginx index page.

### Play around
docker container exec -ti mapped_nginx bash
echo "HI!!" >> /usr/share/nginx/html/index.html 

