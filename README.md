# Docker Testing on WSL 2 (Debian)

This is a very basic report on Docker Engine, nothing beyond fundamental container usage and some networking.


Tried to use the script utility but didn't like the result, so I've just come up with a plain README.



## Installing Docker

#### Script that helps install the best Docker version for your environment
```sh
curl -fsSL https://get.docker.com | bash
```

#### Set Docker daemon in rootless mode
```sh
dockerd-rootless-setuptool.sh install  
```

#### May need this for setting up too
```sh
sudo apt-get install -y uidmap
```

#### Displays running containers and info
```sh
docker container ls
```
##### OR
```sh
docker ps
```

#### If everything is right, test fetching from the hub with a hello-world image
```sh
docker container run hello-world
```



## Basic Commands

#### Displays all containers on the host
```sh
docker container ls -a
```
##### OR
```sh
docker ps -a
```

#### Fetch and run Ubuntu image with an interactive terminal
```sh
docker container run -it ubuntu
```

##### Do this inside the container:
```sh
uptime
```
**NOTE:** Pressing `Ctrl+D` will stop the container since the main process is `/bin/bash`. Instead, press `Ctrl+P+Q` to detach without stopping.

#### Enter the container again
```sh
docker container attach <your_ubuntu_container_ID>
```

#### Displays running processes inside the container
```sh
ps -ef
```

#### Run an image with a specific name
```sh
docker container run --name your_container ubuntu
```

#### Alternatively, create a container without starting it
```sh
docker container create --name your_container <image>
```

#### Start an existing container
```sh
docker container start your_container
```

#### Stop a running container
```sh
docker container stop <container_name>
```



## LAB:

### - Attach to a Ubuntu container and run `watch date`, it will display the current time (and date) every 2 sec.
### - Open another terminal and try to control the container with:
  ```sh
  docker container pause <container_name>
  docker container unpause <container_name>
  ```
### - Alternatively, if you're using WSL and encounter difficulties, try just doing start/stop commands.

### **NOTE:** This happens because WSL includes a translation layer between Linux and Windows resource management systems. WSL2 does not fully implement all Linux kernel features, particularly certain cgroup controllers like the freezer subsystem needed for container pausing.



#### Remove a stopped container
```sh
docker container rm <container_name>
```

#### Display real-time container resource usage statistics
```sh
docker container stats
```

#### Show detailed information about a container or image
```sh
docker inspect <target>
```

#### Show logs from a container
```sh
docker container logs <container_name>
```

#### Fetch a specific image to your host without running it
```sh
docker pull centos:7
```

#### Display all images
```sh
docker image ls -a
```



## Container Management & Networking

#### Run container in detached mode with `-d` flag
```sh
docker container run -d --name my_nginx nginx
```

**NOTE:** When starting this container, there's no bash because the main process in the Nginx image isn't `/bin/bash`.

#### Check running containers
```sh
docker ps
```

#### Execute commands in a running container without attaching
```sh
docker container exec -ti my_nginx ls  
```

#### Start a bash shell inside the running container
```sh
docker container exec -ti my_nginx bash  
```

#### Running this command outside the Nginx container to see related processes on the host
```sh
ps aux | grep nginx
```

### Split terminal view example with two previous commands:
![image](https://github.com/user-attachments/assets/3781c292-3493-4132-9f1e-3cfef9bf91a7)



#### Map your host's port 8080 to the Nginx container port 80
```sh
docker container run -d -p 8080:80 --name mapped_nginx nginx
```

### Now you can access [http://localhost:8080](http://localhost:8080) in your browser to see the Nginx index page.

#### Play around
```sh
docker container exec -ti mapped_nginx bash
echo "HI!!" >> /usr/share/nginx/html/index.html
