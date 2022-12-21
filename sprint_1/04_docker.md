# Hands-On: Introduction to Containers (Docker and Minikube)

> **Tasks**:

> - [Task 0: Prerequisites](#Task_0)
> - [Task 1: Run simple containers](#Task_1)
> - [Task 2: Package your custom application using Docker](#Task_2)
> - [Task 3: Running your application on Kubernetes locally](#Task_3)

## <a name="task0"></a>Task 0: Prerequisites

First, you need to install Docker and Minikube.

### Install Docker Desktop on Mac

[Docker Documentation](https://docs.docker.com/desktop/mac/install/)

### Install Docker Engine with Homebrew

Download and install both the docker-machine and virtualbox packages. Docker requires both of these to run correctly on macOS.

```
brew install --cask docker
brew install docker-machine
brew install --cask virtualbox
```

Then launch the Docker app and confirm your password for privileged access.

### Install Docker Desktop with Homebrew

```
#Install Docker Desktop
brew install --cask docker

#Run Docker Desktop
open /Applications/Docker.app
```

### Install Minikube

To install the latest minikube stable release on x86-64 macOS using binary download:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

### Install Minikube with Homebrew

`brew install minikube`

Check if minikube was installed:

`which minikube`

If which minikube fails after installation via brew, you may have to remove the old minikube links and link the newly installed binary:

```
brew unlink minikube
brew link minikube
```

### Install kubectl

kubectl - the Kubernetes command-line tool allows you to run commands against Kubernetes clusters.

`curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"`

Make the kubectl binary executable.

`chmod +x ./kubectl`

Move the kubectl binary to a file location on your system PATH.

```
sudo mv ./kubectl /usr/local/bin/kubectl
sudo chown root: /usr/local/bin/kubectl
```

### Install kubectl with Homebrew on macOS

`brew install kubectl`

Check if kubectl was installed:

`kubectl version --client`

## <a name="Task_1"></a>Task 1: Run simple containers

### Run a simple nginx container

1. Check if Docker was installed successfully:

`docker version`

2. Run the following command:

```
docker run -d -p 80:80 --name webserver nginx
```

When `nginx:latest` image could not be found locally, Docker automatically _pulls_ it form Docker Hub.

You’ll notice a few flags being used. Here’s some more info on them:

- -d - run the container in detached mode (in the background)
- -p 80:80 - map port 80 of the host to port 80 in the container
- nginx - the image to use

3. Try to access a container on localhost:80 on your machine: http://localhost:80/.

4. Check how long it will take to start a new container with a new image:

`docker run -d -p 8080:80 --name webserver2 nginx`

The image was downloaded already, so this container started faster.

5. You can run bash commands inside a Linux container:

`docker exec -it webserver /bin/bash`

docker exec runs a command in a running container and the switch -it opens an interactive console

6. Stop containers and remove the image:

```
docker stop webserver && docker stop webserver2
docker rm webserver && docker rm webserver2
Docker rmi <image name>
```

## <a name="Task_2"></a>Task 2: Package your custom application using Docker

You can create an image for your application by using Dockerfile. It contains a list of instructions to build images such as installing a package, downloading source code, using a base image.

1. Create a new folder:

```
mkdir new-image
cd new-image
```

**Dockerfile** is a text file that contains a list of instructions needed to build a given image. For example: install a package, download source code, use a base image.

Create a Dockerfile and add the following:

```
FROM nginx
COPY . /usr/share/nginx/html
```

- [FROM](https://docs.docker.com/engine/reference/builder/#from) specifies the base image to use as the starting point for this new image you're creating.
- [RUN](https://docs.docker.com/engine/reference/builder/#run) Executes a command on top of an existing layer and creates a new resulting layer
- [COPY](https://docs.docker.com/engine/reference/builder/#copy) Copies files and directories from a source (host executing build command) to a destination (Container image)

2. Run `docker image build` command to create a new Docker image using the instructions in your Dockerfile.

- `--tag` allows us to give the image a custom name. It usually consists of your Docker Hub username, the application name, and a version.
- `.` tells Docker to use the current directory as the build context

Be sure to include period (`.`) at the end of the command.

```
$ docker build -t <image-name> --tag <yourusername>/<image-name>:1.0 .
```

3. Push your image to Docker Hub

List local images:

`docker images`

Log in to Docker Hub:

`docker login --username <yourusername>`

Push the image to your repository in Docker Hub:
`docker push yourusername/nginx:1.00`

## <a name="Task_3"></a>Task 3: Running your application on Kubernetes locally

1. Start your cluster:

`minikube start`

After the cluster is started, you can verify its status.

`minikube status`

2. See pods in your cluster:

`kubectl get po -A`

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

3. Create a first deployment

**Deployments** represent a set of multiple, identical Pods with no unique identities. A Deployment runs multiple replicas of your application and automatically replaces any instances that fail or become unresponsive.

4. Create a sample deployment and expose it on port 8080:

```
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
```

**Service** is an abstract way to expose an application running on a set of Pods as a network service.

5. Expose deployment as a Service:

```
kubectl expose deployment web --type=NodePort --port=8080
```

6. It may take a moment, but your deployment will soon show up when you run:

```
kubectl get services web
```

The easiest way to access this service is to let minikube launch a web browser for you:

```
minikube service web
```

7. Delete deployment and service:

```
kubectl delete deployment web
kubectl delete service web
```

8. Delete your Minikube cluster:

```
minikube delete
```
