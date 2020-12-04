# Basic Docker Commands
The basic commands for Docker

## Table of contents:  
* [The Basics](#the-basics)
* [Stop and Remove](#stop-and-remove) 
* [docker-compose](#docker-compose)  
<br>

### The Basics

In order to use Docker, you must have Docker installed. To check the Docker options:
```
$ docker
```

It will show a list of options. It is a proof that you have Docker installed in the computer and ready to pull **images** from <a href="https://hub.docker.com"><b>Docker Hub</b></a> or repository.

Here we pull the image of the popular application, <a href="https://hub.docker.com/r/jupyter/datascience-notebook">*jupyter/datascience-notebook*</a>, from <b>Docker Hub</b> and run it into a <b>container</b> as an example.
Look for the *Docker Pull Command* section. Copy the command and execute it to download the **image**:
```
$ docker pull jupyter/datascience-notebook
```

* The storage location of Docker **image**: 
	+ Ubuntu: `/var/lib/docker/`
	+ Fedora: `/var/lib/docker/`
	+ Debian: `/var/lib/docker/`
	+ Windows: `C:\ProgramData\DockerDesktop`
	+ MacOS: `~/Library/Containers/com.docker.docker/Data/vms/0/`

To see what **images** do you have in the machine:
```
$ docker images
```

To pull down an **image** with the specific version in the tag (ex: latest / 1.0.0):
```
$ docker pull <image>:<tag>
```
Now, the **image** we just downloaded is at rest in the machine. We need to run the **image** of application in a container:
```
$ docker run -d -p 8888:8888 --name jupyter1 jupyter/datascience-notebook
```

* The terminal returns an ID of the container. 
* The `-d` option runs the **container** in the background. 
* The `-p` option tells Docker that the container is running port to **8888** inside the **image** (right) and exposing the port to **8888** outside the **image** (left)
	+ **8888** is publicly accessible TCP port
* `jupyter1` is the name of the **container** with the `--name` option
* `jupyter/datascience-notebook` is the **image** name


To check the current running **container**:
```
$ docker ps -a
```

* The `-a` option shows all the **containers**, including the stopped **containers** 

You will see a **container** is currently running with the ID that was returned from the `docker run` command with container name, *jupyter1*.

To see the log of the specific running **container**:
```
$ docker logs jupyter1
```

* The log of the **container** will be shown
* An URL of the application will be produced
	+ Without exposing the ports externally, the link is not gonna work


By using the link of the application, we can run the jupyternotebook application without installing Anaconda platform.

The data created in the **image** is temporary. To make it permanent, you can add `-v` option to mount a volume (or a folder) when you create a **container**:
```
$ docker run -d -p 8888:8888 -v jupyter-data:/home/jovyan/ --name jupyter1 jupyter/datascience-notebook
```

* The `-d` option runs the **container** in the background. 
* The `-p` option tells Docker that the container is running port to **8888** inside the **image** (right) and exposing the port to **8888** outside the **image** (left)
* `-v` shows the internal path and external path that data is going to be stored
* `jupyter-data` is the name of the volume that is stored outside of the **image**
* `/home/jovyan/` is the path that data is stored inside the **container** based on the jupyter/datascience-notebook documentation

To check a list of volume in the machine:
```
$ docker volumne ls
```

<br>

### Stop and Remove

To stop running a **container**:
```
$ docker stop jupyter1
```

To re-running a **container**:
```
$ docker start jupyter1
```

The **container** is currently stop running but it is waiting in the backgroud. To remove the **container**:
```
$ docker rm jupyter1
```

To remove an **image**:
```
$ docker rmi <image id>
```

* The **image** must not be used by any **container** (running/stopped)

<br>

### docker-compose

Oftentime, we feel tedious typing `docker run`, `docker stop` and `docker rm` commands. And `docker-compose` is a solution of it. It is a means to take several **containers** and get them to come up and down together. It can also have them talked to each other.  

To do so, you have to make a `docker-compose` file, `docker-compose.yml`:
```
version: '<1.0.0>'

services:
  <jupyter>:
    image: <jupyter/datascience-notebook>
    ports:
        - <8888>:<8888>
    volumes:
        - <./data/>:</home/jovyan/>

```

* The Compose file is a <a href="https://yaml.org/">YAML</a> file defining <a href="https://docs.docker.com/compose/compose-file/#service-configuration-reference">services</a>, <a href="https://docs.docker.com/compose/compose-file/#network-configuration-reference">networks</a> and <a href="https://docs.docker.com/compose/compose-file/#volume-configuration-reference">volumes</a>
* The default path for a Compose file is `./docker-compose.yml`
* The tab characters to indent is not permitted in a YAML file

Once you have the `docker-compose.yml`, redirect to the path that the docker file is stored in by using `cd` command. Then, you can use the docker-compose command to run the **image** in a **container**:
```
$ docker-compose up -d
```

To veiw what **container** is running:
```
docker-compose ps
```

* Docker will name the **container** based on the folder which the `docker-compose.yml` is store in.

To veiw the logs of **container** without specifiying names:
```
docker-compose logs
```

To stop running a **container**:
```
docker-compose stop
```

To remove a **container**:
```
docker-compose down
```

* It first stop the **container**, and then remove the **container**

<br>

### Reference:  
* [Let's Learn Docker by Michael Fudge](https://www.youtube.com/watch?v=fQORO9QEJN4&t)  
* [Docker Documentation](https://docs.docker.com/)