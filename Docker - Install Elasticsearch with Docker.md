# Install Elasticsearch with Docker, start , then stop

**Certainly! Let’s install Elasticsearch using Docker, start it, and then stop it. Here are the steps:**

1. Install Docker:
	- First, make sure you have Docker installed on your system. 
	- If you haven’t already, follow the instructions to Get Docker. 
	- If you’re using Docker Desktop, allocate at least 4GB of memory in the Docker settings.
2. Create a New Docker Network:

 	```
	docker network create elastic
  	```

3. Pull the Elasticsearch Docker Image: Pull the official Elasticsearch image:
	```
	docker pull docker.elastic.co/elasticsearch/elasticsearch:8.12.2
	```
4. Start an Elasticsearch Container: Run the following command to start a single-node Elasticsearch cluster:
	
	 ``` 
	 docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.12.2
	```
 
	-Use the -m flag to set a memory limit for the container. This removes the need to manually set the JVM size.
	-The command will print the elastic user password and an enrollment token for Kibana.
	-Copy the generated elastic password and enrollment token. These credentials are only shown when you start Elasticsearch for the first time. You can regenerate the credentials using the following commands:

	```
	docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
	docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
	```
5. Copy the SSL Certificate: Copy the http_ca.crt SSL certificate from the container to your local machine:
	
 	```
	docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
	```
  
6. Verify Elasticsearch: Make a REST API call to Elasticsearch to ensure the container is running:
	
 	```
  	curl http://localhost:9200/ 
   	```
    
7. Stop the Elasticsearch Container: When you’re done testing, stop the container:

 	```
  	docker stop es01
   	```

That’s it! You’ve successfully installed, started, and stopped Elasticsearch using Docker. 🚀
