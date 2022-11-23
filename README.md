# docker-github-runner-linux

Repository for building a self hosted GitHub runner as a ubuntu linux container

For more details on using this repo, check out my blog post: [Self Hosted GitHub Runners on Azure - Linux Container](https://dev.to/pwd9000/create-a-docker-based-self-hosted-github-runner-linux-container-48dh).

Also see my GitHub repository: [docker-github-runner-windows](https://github.com/Pwd9000-ML/docker-github-runner-windows) for building a self hosted GitHub runner as a windows container. 

# How to use

## Using Docker compose (preferred)
### Set environment variables for parameters
```
#set system environment with $env: 
#execute the following in Powershell
$env:GH_OWNER='Org/Owner'
$env:GH_REPOSITORY='Repository'
$env:GH_TOKEN='myPatToken'
```
### Build Docker image using Compose
```
docker-compose build
```
### Run image using Compose
Run image and scale to 3 containers
```
docker-compose up --scale runner=3 -d
```
### Destroy images using Compose
```
docker-compose stop
docker rm $(docker ps -aq)
```

## Using Docker
### Build Docker image
```
docker build --build-arg RUNNER_VERSION=2.299.1 --tag docker-github-runner-linux .
```
[Create PAT token](https://docs.github.com/en/enterprise-server@3.4/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

The minimum permission scopes required on the PAT token to register a self hosted runner are:
- "repo"
- "read:org"
  
### Run image
```
docker run -e GH_TOKEN='myPatToken' -e GH_OWNER='orgName' -e GH_REPOSITORY='repoName' -d image-name
```
You can repeat this command to spin up addittional runners.

### Destroy images
```
docker stop $(docker ps -aq) && docker rm $(docker ps -aq)
```

