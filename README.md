# Linuxengineering's docker compose expo

Collection of docker-compose files optimized to launch fast and easy.

Some unit requires extra files to launch therefore helper script is created to
manage the lifecycle of unit.

## Managing units

Every unit has its own ./expo script which "inherits" the common operations
(up, destroy, init, etc ) so they can be managed by "standard"
operations. the ./expo script located in the root directory has a dispatcher
function to manage units. Units can be managed two ways:
```bash
./expo unit gitlab up
# or
./gitlab/expo up
```

## Command cheat sheet

### Get help

```bash
$ ./expo
It is a wrapper script wherein you can collect tiny commands.
Specify one helper function from the followings:
	help - Returns help with the list of all annotated functions
	ips - Print the ip adresses of running containers
	unit - Manage single unit
	units_list - List all units
```

### List units with short summary

```bash
./expo units_info
frigate - Web based NVR object recogniser and data collector
gitlab - Powerful enterprise ready web based multi project git CI/CD platform with artifactory, user and issue management and more.
redmine - Web multi project issue base project management system
sonarqube - Static code analyzer web service covers majority of programming languages
```

### Start unit
```bash
./expo unit gitlab up
[+] Running 3/3
 ✔ Network gitlab_default            Created                                                                                                                           0.2s
 ✔ Container gitlab-gitlab-1         Started                                                                                                                           0.3s
 ✔ Container gitlab-gitlab-runner-1  Started                                                 
```
`up` is a shortcut to run `init` and `start` sequentially

### List unit services
```bash
./expo unit gitlab dc ps
NAME                     IMAGE                         COMMAND                  SERVICE         CREATED         STATUS                            PORTS
gitlab-gitlab-1          gitlab/gitlab-ce:latest       "/assets/wrapper"        gitlab          4 seconds ago   Up 4 seconds (health: starting)   22/tcp, 0.0.0.0:32941->80/tcp, :::32941->80/tcp, 0.0.0.0:32942->443/tcp, :::32942->443/tcp
gitlab-gitlab-runner-1   gitlab/gitlab-runner:latest   "/usr/bin/dumb-init …"   gitlab-runner   4 seconds ago   Up 4 seconds     
```

`dc` is a shortcut to run `docker compose "$@"` in the unit's directory.  
Calling `... dc ps` is useful to figure out host port bindings and inspecting service
health status.

### Destroy unit
```bash
./expo unit gitlab destroy
[+] Running 3/3
 ✔ Container gitlab-gitlab-runner-1  Removed                                                                                                                           0.3s
 ✔ Container gitlab-gitlab-1         Removed                                                                                                                          10.8s
 ✔ Network gitlab_default            Removed                                               
```
`destroy` is a shortcut to run `down` and `purge` sequentially

### Container ips
Get the ip address of ll running container.
If you use bare docker installation (unlike docker desktop where docker
  network(s) are fully separated) you can access container services by ip
  adresses.
```bash
./expo ips
sonarqube-sonarqube-1  172.24.0.3
sonarqube-db-1  172.24.0.2
redmine-redmine-1 172.23.0.3  
redmine-mysql-1  172.23.0.2
frigate-mqtt-1  172.22.0.2
frigate-frigate-1  172.22.0.3
gitlab-gitlab-runner-1  172.18.0.3
gitlab-gitlab-1  172.18.0.2
openwrt-openwrt-1  172.21.0.2
zabbix-zabbix-java-gateway-1  172.20.0.4
zabbix-mysql-server-1  172.20.0.5
zabbix-zabbix-web-1  172.20.0.3
zabbix-zabbix-server-1  172.20.0.2
docker-swarm-manager-1  172.19.0.3
docker-swarm-worker02-1  172.19.0.4
docker-swarm-worker01-1  172.19.0.2
```
Eg.: redmine at http://172.23.0.3:3000/

## Unit rules
###  docker-compose.yml
- privileged: must be avoided, visit README.md first
- volumes: instead of volumes, use mounts to ./data/$SERVICE/$PURPOSE
- restart: unless-stopped
- fields not set:
  - container_name
  - ports with fixed ingress (but used ports noted)

### What can you find in unit specific README.md ?
Note:
- noting usage of privileged when it is inevitable to use
- necessary environment variables to set
- bootstrap script when necessary to start
- necessary configuration
- default user and password
- post start configuration steps
