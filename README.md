# Kubernetes Tools

Swiss army knife for k8s infra tools. Forked from: shavo007/docker-kops
Production Grade K8s Installation, Upgrades, and Management

## Includes
* Kops
* Kubectl
* AWS CLI
* Terraform
* HELM

```bash
(sample vars)
export NAME=k8s.shanelee.name.k8s.local
export KOPS_STATE_STORE=s3://shane-k8s-bucket
export KUBECONFIG=~/stacks/${NAME}/kubeconfig
export AWS_REGION=ap-southeast-2

docker run --rm -it \
  -e AWS_REGION \
  -e KUBECONFIG \
  -e AWS_PROFILE \
  -e KOPS_STATE_STORE \
  -v "$HOME"/.ssh:/root/.ssh:ro \
  -v "$HOME"/.aws:/root/.aws:ro \
  -v "$HOME"/.kube:/root/.kube:rw \
  -v "$(pwd)":/workdir \
  -w /workdir \
  shanelee007/alpine-kops

```

## Running on Windows 7

It is difficult to setup and run KOPS, KUBECTL etc on a Windows box. Use this docker image 

### Prerequisite

#### Mount local files to docker-machine

Share your c/d drive folder to the Docker machine VM as shown below (eg., c:\projects is mounted as /projects):

![Shared Folder](doc/SharedFolder.png "SharedFolder")

Restart the docker-machine with following command

````
docker-machine restart
````

You can now mount the folder as volume when you run the container.
````
-v /projects:/data 
````

#### AWSCLI [OPTIONAL]

Install AWS CLI and configure your AWS access key and Secret key

### Steps

#### Starting the container

Open Docker CLI through Kitematic

Run the following if you have AWS CLI already configured
````
$AWS_ACCESS_KEY_ID = aws --profile default configure get aws_access_key_id
$AWS_SECRET_ACCESS_KEY= aws --profile default configure get aws_secret_access_key
docker run -it --rm -p 80:80 -v /projects:/data -e NAME=atlas.rts.onl -e KOPS_STATE_STORE=s3://alcyon-k8s-bucket -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY shanelee007/alpine-kops:latest
````

Else, set the access key and secret key in the following command
````
docker run -it --rm -p 80:80 -v /project:/data -e AWS_ACCESS_KEY_ID=<AWS_ACCESS_KEY_ID> -e AWS_SECRET_ACCESS_KEY=<AWS_SECRET_ACCESS_KEY> -e NAME=atlas.rts.onl -e KOPS_STATE_STORE=s3://alcyon-k8s-bucket  shanelee007/alpine-kops:latest
````

In the container, run the following commands to check whether you are able to connect with the K8s cluster.
```` 
kops export kubecfg atlas.rts.onl
kubectl get nodes  
````