- config env 
	- copy .env.example to .env
	- modify the path 
- start docker
	- grep -qxF 'export COMPOSE_FILE=$HOME/ai_project/docker/docker-compose.yml' ~/.bashrc || \
echo 'export COMPOSE_FILE=$HOME/ai_project/docker/docker-compose.yml' >> ~/.bashrc
	- docker compose -f ${COMPOSE_FILE} up -d
- enter docker 
	- docker compose exec dev bash
- exit docker
	- exit
- stop and del container
	- docker compose down
	
