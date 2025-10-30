[//]: # "Install modern build system used by docker"
xbps-install -S buildx

[//]: # "Disable properly a local installation or leftover docker container which might already use 3306 port"
sudo sv stop (mysqld / mariadb)
sudo rm /var/service/mysql

[//]: # "... or map to another port to avoid conflict with existing data base instance into docker-compose manifesto"
ports:
  - '3307:3306' # host:container

[//]: # "Pull the microsoft SQL server docker image"
docker pull mcr.microsoft.com/mssql/server:2022-latest

[//]: # "Run SQL Server in a container"
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=YourStrong!Password123' -p 1433:1433 --name sqlserver --hostname sqlserver -d mcr.microsoft.com/mssql/server:2022-latest

[//]: # "Check all active/running containers"
docker -ps [-a]

[//]: # "Check docker network if port 1433 is not listening"
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' sqlserver

[//]: # "Start / restart / stop / remove a docker container"
docker { start / restart / stop / rm } sqlserver

# Step 1: Check container is running
docker ps | grep <container_id_or_name>

# Step 2: Check logs for successful startup
docker logs sqlserver | grep -i "SQL Server is now ready"

# Step 3: Check port mapping
docker port sqlserver

# Step 4: Check host port listening (super user can access instances' name)
netstat -tln | grep 1433
# OR
[sudo] ss -tulpn | grep ':1433'

[//]: # "Check if a container is restart at system boot (may return `always`)"
docker inspect <id_or_name> --format='{{.HostConfig.RestartPolicy.Name}}'

[//]: # "Disable automatic restart without removing or stopping this container"
docker update --restart=no <id_or_name>

[//]: # "Create/start containers; re-create them if config changed"
docker compose restart [service_name]

[//]: # "Create/start containers; re-create them if config changed (images are already pre-built)"
docker compose [--build] up -d

[//]: # "Full stop and start (fresh state)"
docker compose down; docker compose up -d

[//]: # "Check if volume exists"
docker volume ls | grep phpmyadmin


[//]: # "Check if network exists"
docker network ls | grep phpmyadmin

[//]: # "Inspect the container's file structure"
docker exec -it phpmyadmin-first-try-dbadmin-1 ls /var/www/html

- The official phpmyadmin/phpmyadmin image is built so that is it installed in the web server's document root.
- Docker credo: "One container, one purpose"