# Docker Compose
Docker Compose is a tool for running multi-container applications on Docker defined using the Compose file format. A Compose file is used to define how one or more containers that make up your application are configured. Once you have a Compose file, you can create and start your application with a single command: docker compose up.

## Install Plugin
1. Installation commands
	```console
	sudo apt-get update
	sudo apt-get install docker-compose-plugin
	```

2. Verify Docker Compose is installed
	```console
	docker compose version
	```

	Expected Output: 
	```
	Docker Compose Version vN.N.N
	```