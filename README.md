# JupyterHub with CUDA and individual single-user containers


## Features

- CUDA-capable using CUDA-capable JupyterLab base containers ([docker-jupyter-cuda](https://github.com/kentwait/docker-jupyter-cuda))
- Spawns single-user JupyterLab containers using [dockerspawner](https://github.com/jupyterhub/dockerspawner)
- Uses [jupyterhub-firstuseauthenticator](https://pypi.org/project/jupyterhub-firstuseauthenticator/) authenticator that 
set a user's password on first login to JupyterHub.


## Usage

- Clone this repository, e.g. `git clone https://github.com/kentwait/jupyterhub-dockerspawner-cuda.git`
- Make a `.env` file in the format `KEY=VALUE` and save it alongside `docker-compose.yaml`
Refer to the Environment variables section for more information.
- Run `docker compose up`


## Environment variables

Make a `.env` file alongside `docker-compose.yaml`.
The following are *required* for the compose file to work properly.
- `COMPOSE_PROJECT_NAME`
Docker compose project name. Example: `jupyterhub`.
- `DOCKER_NETWORK_NAME`
Name of the internal docker network to be used by JupyterHub and JupyterLab container instances.
Example: jupyterhub_default
- `HUB_VERSION`
JupyterHub version to use. Example: `4.0.2`
- `HUB_BASE_IMAGE_NAME`
Container name or URL to pull the base container from to build the JupyterHub container.
The default JupyterHub container is at `jupyterhub/jupyterhub`.
- `HUB_BASE_IMAGE_VERSION`
Base container version. If using the default JupyterHub container, this is the same as `HUB_VERSION`.
Instead of manually writing the version again, you can set it to `HUB_BASE_IMAGE_VERSION=${HUB_VERSION}`.
- `HUB_IMAGE`
Name of the JupyterHub image built by Docker. Example: `jupyterhub_img`
- `HUB_HOST_PORT`
Port outside Docker to access JupyterHub web interface.
This is the first value when port-forwarding in Docker using `-p`. Example: `8000`.
- `HUB_CONTAINER_PORT`
Port of the JupyterHub container.
This is the second value when port-forwarding in Docker using `-p`. Example: `8000`.
- `HUB_CONTAINER_NAME`
Name of the JupyterHub container in Docker. Example: `jupyterhub`
- `LAB_VERSION`
JupyterLab version to be used by single-user containers.
Example: `4.0.10` 
- `LAB_BASE_IMAGE_NAME`
Container name of URL to pull the base container from to build the JupyterLab container.
It is recommended to use containers derived from [Jupyter Docker Stacks](https://github.com/jupyter/docker-stacks/tree/main).
For CUDA-capable JupyterLab images, you can use images from [docker-jupyter-cuda](https://github.com/kentwait/docker-jupyter-cuda).
Example: `kentwait/pytorch-notebook-cuda`
- `LAB_BASE_IMAGE_VERSION`
Base container version. Example: `latest` or `cuda11.7.1-cudnn8-devel-ubuntu22.04-py3.9`
- `LAB_IMAGE`
Name of the JupyterLab image built by Docker. Example: `jupyterlab_img`

The following are *optional* variables.
- `LAB_CONTAINER_PORT`
Port for individual single-user JupyterLab containers.
Value of `0` means Docker will randomly assign a port.
Specifying a value (example: `8001`) means all single-user containers will be using this port.  
Default: `0`
- `LAB_RUNTIME`
Used to specify the `nvidia` runtime for CUDA-capable JupyterLab containers.
Note that specifying `nvidia` to non-CUDA JupyterLab images will not make them CUDA capable.
Example: `docker` for non-CUDA and `nvidia` for CUDA.  
Default is unset
- `LAB_BIND_MOUNTS`
List of bind mounts to mount filesystem directories into single-user JupyterLab containers separated by commas.
It is strongly advised not to bind mount the single-user base directory `/home/jovyan`.
Instead, bind mount to subdirectories such as `/home/jovyan/something`.
Example: `/mnt/hdd:/home/jovyan/hdd,/home/{username}:/home/jovyan/local_home`  
Default is unset
- `ALLOWED_USERS`
List of allowed whitelisted users separated by commas.
On first login, these users can freely set their password and start a JupyterLab instance.
If empty, only the user `admin` is allowed to login after the creating the JupyterHub container.
Example: `user1,user2,user3`  
Default is unset
- `IDLE_CULLER_TIMEOUT`
Amount of idle time in seconds before single-user JupyterLab containers are automatically stopped.
Default: `3600`


## Credits

Derived from the following:
- [Deploying a Containerized Jupyterhub Server with Docker](https://opendreamkit.org/2018/10/17/jupyterhub-docker/)
- [Deploying Jupyterhub using Docker (PDF)](https://www.pharmasug.org/proceedings/china2022/AT/Pharmasug-China-2022-AT153.pdf)
- [How to Create a GPU-Powered Containerized Multi-User JupyterHub Research Server?](https://tustunkok.github.io/tutorial/notes-to-myself/vps/2020/05/16/how-to-create-a-gpu-powered-containerized-multi-user-jupyterhub-research-server.html)


## See also

- [DockerSpawner docs](https://jupyterhub-dockerspawner.readthedocs.io/en/latest/index.html)
- [JupyterHub - Spawners and single-user notebook servers](https://jupyterhub.readthedocs.io/en/stable/tutorial/getting-started/spawners-basics.html)
