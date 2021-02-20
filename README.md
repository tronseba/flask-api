# FLASK-API 

This is the code and scripts to develop and deploy FLASK-API Challenge application

## Required tools
* ansible
* kubectl
* helm
* docker

## Build Container

Execute the following commando inside script directory to build the docker image:
```bash
ansible-playbook build.yaml -vv
```

If you like to build the image and push the it into the registry defined in default-vars.yaml

```bash
ansible-playbook build.yaml --extra-vars push_image=yes -vv
```
Make sure to be logged in into the docker registry before to execute the script

## Configure Modules to be installed

On scripts/default-vars.yaml edit the modules variables with the list of modules to be installed during container build process: 

```yaml
# Python Module list
modules: "flask redis"
```

## Configure registry

Set registry information and image upstream name by changing variables in default-vars.yaml

```yaml
#regidtry base url
docker_registry: "xxx.xxx.com"

#registry namespace o tenant
registry_namespace: "default_namespace"

#docker image name
image_name: "flask-api"
```

Note that local image will always be tagged as **flask-api:latest** registry variables are only using when you try to push the image to external registry

## Run application locally

Use docker compose command inside **container** directory in order to start or stop the application locally on production or development mode. Development mode allows you mo mount src directory in order to edit it in real time (with out rebuild de image. Production mode will use the source conde included during the build process. 

### Start Development mode:
```bash
docker-compose -f docker-compose-dev.yaml up 
#press ctrl+c to stop the container
```

### Start Production mode:
```bash
docker-compose -f docker-compose.yaml up 
#press ctrl+c to stop the container
```

## Helm Chart

Use the following command to deploy the application on kubernetes cluster or minikube, edit attributes on values.yaml file inside the helm chart flask-api to change the image registry or ingress atributes. For minikube default values will use the local application image.

```bash
 # to install the release
 helm install <release_name> ./flask-api
 # to delete the release 
 helm delete <release-name>
 # to forward the application locally whit out use ingress
 kubectl port-forward service/<release_name>-flask-api <local_port>:8080
```

# NOTES:

Do not include any secret on this public repository, you can reference to secrets inside the helm chart, but no credentials or secrets must be shared inside this code.

# TODO:

* Enable redis cluster
* Configure redis credentials