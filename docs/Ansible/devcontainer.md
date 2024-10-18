# Dev Container

To speed up development on ansible i have created a dev container in visual studio using a docker backend, this allows you run ansible on any system.
  ## Prerequisites
  

 - [Docker](https://www.docker.com/products/docker-desktop/)
 - [Visual Studio Code](https://code.visualstudio.com/)
 - [Dev container](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

Once you have installed the prerequisites you can follow allong with this turtorial.

### Let's get started:
Create a new directory on your system since i am creating this turtorial on windows this wil live in c:\temp i name my project dev_container.

    mkdir c:\temp\dev_container
    cd c:\temp\dev_container
    code . 

The last command wil start visual studio code inside the folder.
now inside the project we will create a new folder called .devcontainer and inside this folder create a new file devcontainer.json.

```json
// For format details, see https://aka.ms/devcontainer.json. For config options, see the
// README at: https://github.com/devcontainers/templates/tree/main/src/docker-existing-dockerfile
{
	"name": "python",
	"build": {
		// Sets the run context to one level up instead of the .devcontainer folder.
		"context": "..",
		// Update the 'dockerFile' property if you aren't using the standard 'Dockerfile' filename.
		"dockerfile": "Dockerfile"
	},
	"forwardPorts": [8080,8080],
	"mounts": [
        //"source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind"
		//"source=project,target="
	],
	//"postCreateCommand": "chmod +x /workspaces/awx/scripts/*.sh",
	"customizations": {
	"vscode": {
			"extensions": [
				"ms-python.python",
				"shd101wyy.markdown-preview-enhanced"
			]
		}
	}
}

This docker file is depending on your needs , in the sample i am using a Python enviroment where the requirements are getting installed (make sure the requirements file is present)
```
### Dockerfile
Add in the same .devcontainer folder a new Dockerfile

```docker
FROM python:3.10-bullseye
LABEL maintainer "Seys Alain"
ENV container "docker"

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt
RUN python3 -m pip install --upgrade pip
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends 
    
```
Now we have a pretty good base to work with to get strated with the dev container you can enter the hotkeys CTRL + SHIFT + P (Devcontainers Rebuild Container). When you do this Visual Studio Code will build the enviroment based on .devcontainer.json and Dockerfile.

When this is ready you can startup a new console and work in your project like you work on a server.

Notice when you need extra dependencies you need to add them to your dockerfile