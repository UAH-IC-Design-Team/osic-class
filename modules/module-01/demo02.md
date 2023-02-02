# demo02 - Git continued and Docker
The following demo requires the completion of HW00-Instalation to follow along.

# Bash Things Missed last time
```

rm 

mv

# & ampersand

fg 

bg

```

# Git

### Remotes

```
git merge conflicts

git clone

git remote add

# SSH Side bar

git remote remove

git push 

git fetch

git pull 


```

# Github Additions
Github has created a few additional integrations to git that are designed especially towards the open source community. These are not git commands, but rather operations that may be performed in the Github website, Github cli or Github desktop app. 

### Forking
Forking is making a full copy of a repository. Forking is analogus to creating a branch, except it is different than a branch because it copies the whole repository (including branches).

### Pull Requests
A pull request is analogus to a merge as forking is analogus to a fork. The differenceis that a merge combines commits, but a pull request combines repositories. 

### Issues
Issues are a convenient way bugs and feature requests. Issues are assigned a number and can be referenced and linked in pull requests and other issues by commenting `#<number>`. For example commenting `fixed issue #23` will reference and link issue `#23`.

#### SSH
SSH is discussed in detail in HW01.
[Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell)

[SSH keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)


# Docker
We have created a [Docker cheatsheet](https://github.com/UAH-IC-Design-Team/documentation/wiki/Docker-Cheat-Sheet) with all of the most common commands and workflows needed for this class.

Docker has excelent documentation that can be found [here](https://docs.docker.com/).

[Docker Hub](https://hub.docker.com/) is the default docker image repository containing many official docker base images such as Ubuntu or Alpine Linux.

### Basic Archetecture


### Docker Container

```
docker container ls

docker container run

docker conainer kill

docker container rm

```

### Dockerfile
```
FROM <image base>

COPY ./local /container

WORKDIR /container

ENV ENV_VAR=value

RUN some_command

EXPOSE <port_num>

ENTRYPOINT ["some", "command"]


```

.dockerignore


### Docker image 
```
docker image ls

docker image build

docker image prune

docker image history

docker image tag

```

### Docker compose
Rather than writing long bash commands, docker compose uses a yml file to execute containers.
[docker compose docs](https://docs.docker.com/compose/features-uses/)

# Makefile
- Targets
- Target dependencies


# Clarification on where to use SSH and git and where to launch the tools...
