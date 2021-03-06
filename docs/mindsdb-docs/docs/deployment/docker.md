# Docker Guide

1. Install Docker

	Install <a href="https://docs.docker.com/install" target="_blank">Docker</a> on your machine. To make sure Docker is successfully installed on your machine, run:

	```
	$ docker run hello-world
	```

	You should see the `Hello from Docker!` message displayed. If not, check Docker's <a href="https://www.docker.com/get-started" target="_blank">Get Started</a> documentation.

	!!! warning "For 'Docker for Mac' users"
		By default, Docker for Mac allocates __2.00GB of RAM__. This is insufficient for deploying MindsDB with docker. We recommend increasing the default RAM limit to __4.00GB__.

		Please refer to Docker's [Docker Desktop for Mac user manual](https://docs.docker.com/desktop/mac/#resources) for more information on how to increase the allocated memory.

1. Start MindsDB

	Run the below command to start MindsDB in Docker:

	```
	docker run -p 47334:47334 -p 47335:47335 mindsdb/mindsdb
	```

	!!! warning "MKL Issues"
		If you experience any issues related to MKL or if your training process does not complete, please add the `MKL_SERVICE_FORCE_INTEL` environment variable:
		```
		docker run -e MKL_SERVICE_FORCE_INTEL=1 -p 47334:47334 -p 47335:47335 mindsdb/mindsdb
		```

	!!! info
		The `-p` flag allows access to MindsDB by two different methods:

		* `-p 47334:47334` - Publish port 47334 to access the MindsDB GUI and HTTP API. 
		* `-p 47335:47335` - Publish port 47335 to access the MindsDB MySQL API.

!!! tip "Optional: Additional Configuration"

	The default configuration for MindsDB's Docker image is represented as a JSON block:
	```
	{
		"config_version":"1.4",
		"storage_dir": "/root/mdb_storage",
		"log":
			{ "level": { "console": "ERROR", "file": "WARNING", "db": "WARNING" } },
		"debug": false, 
		"integrations": {}, 
		"api": 
			{ 
				"http": { "host": "0.0.0.0", "port": "47334" }, 
				"mysql": { "host": "0.0.0.0", "password": "", "port": "47335", "user": "mindsdb", "database": "mindsdb", "ssl": true }, 
				"mongodb": { "host": "0.0.0.0", "port": "47336", "database": "mindsdb" } 
			}
		}
	}
	```

	(Generated by [this template](https://github.com/mindsdb/mindsdb/blob/stable/docker/dockerfile_release.template).)

	To override the default configuration, you can provide values via the MDB_CONFIG_CONTENT environment variable:

	```
	docker run -e MDB_CONFIG_CONTENT='{"api":{"http": {"host": "0.0.0.0","port": "8080"}}}' mindsdb/mindsdb
	```