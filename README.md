## 1. Install Docker Desktop
If Docker is not already installed, download it for your device at:
https://www.docker.com/get-started/

Before continuing, open Docker and make sure it is running.

## 2. Download this repository's files
In order to continue to create the Docker image and container, download this repository's `jupyter-testing-main` file.

Before continuing, make sure you are in the `jupyter-testing-main` directory in your device (you can change the name of this directory if you like).

In `jupyter-testing-main` there is a Dockerfile that contains the instructions for Docker to create an ubuntu-based image; this can be changed, but with caution as the changes may not work with how I have written it (**you can leave this alone though**, I believe it should work properly with any jupyter notebook). 

There is also a `requirements.txt` that contains the names of the necessary libraries to run the jupyter notebook inside `project`. If changing the notebook inside `project` with your own, make sure `requirements.txt` contains all the libraries you need so that Docker downloads them into the image. The libraries **must have the name you would use when using `pip install <library>`** as pip is what my Dockerfile has specified as its installer.

Inside `project` exists my jupyter notebook and a picklefile. These can be removed and replaced with a jupyter notebook of your choice and any accompanying files your project will need. 

## 3. Creating the image

**If wanting to upload image to Docker, have your `<image_name>` be `your_docker_username/your_repository_name` in order to upload to Docker Hub** 

Use the following command to build a docker image of name `<image_name>`:
```
docker build -t <image_name> .
```

The `-t` flag specifies that we want to give the image a specific name `<image_name>`. 

The `.` at the end of the command specifies the directory in which the *Dockerfile* lies. In our case this is our current working directory which we specify with a dot.

## 4. Start the container
Use the following command to start the container:
```
docker run -it -p 8888:8888 -v "$(pwd)/project:/project" <image_name>
```

The `-it` flag specifies to run the container in interactive mode (i.e. printing the output of the container to our shell). 

The `-p` flag specifies that we map port 8888 of the container (the one after the :) to port 8888 of our host machine (the one before the :). It effectively redirects our localhost:8888 to the localhost:8888 of the container which is where the notebook server is running.

The `-v` flag specifies that we want to share a folder with the container, meaning that files can be exchanged instantly and also persist on our host machine after the container is stopped. The part before the `:` specifies the folder *project* in our currend working directory (hence `$(pwd)`) as our endpoint of the folder sharing. The part after the `:` specifies the absolute path of the respective endpoint in the container. It is also the directory the container is already using (as specified in the Dockerfile) which means that all files in the project folder will be visible when starting jupyter notebook. 

The `<image_name>` at the end is the name of the image we want to use.

## 4. Accessing the notebook
The container prints the jupyter kernel log into the terminal. Opening the link `http://127.0.0.1:8888/?token=...` in a browser opens the frontend of the jupyter server created by the container. You can now simply use jupyter notebooks as if it ran on your on machine (which it still does, technically, but the container does not know that and therefore we must act as if it didn't).

## 5. Stopping the Container
Simply press `CTRL+C` in the terminal displaying the jupyter notebook kernel and confirm with `y`. This should stop the container. If it does not, open another terminal, run `docker ps` to get a list of running containers and their respective names. Then run `docker stop <container_name>`. The container name is something like *stoic_hamilton* and is automatically assigned to a container when it is started. If that also does not work, run `docker kill <container_name>`. 

## 6. Opening container terminal
Executing `docker run` with this image will turn the terminal into the output of the jupyter kernel where you won't be able to enter futher commands. Should you want to enter the terminal of the running container, execute 
```
docker ps
```
to get a list of running containers and their respective names. Then execute the following command to get into the container:
```
docker exec -it <container_name> /bin/bash
```
## 7. Pushing image to Docker Hub

use the following command

```
docker push your_docker_username/your_repository_name
```
