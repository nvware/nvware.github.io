# Graylog
https://go2docs.graylog.org/5-0/downloading_and_installing_graylog/docker_installation.htm?tocpath=Downloading%20and%20Installing%20Graylog%7CInstalling%20Graylog%7C_____2

**Docker**
Docker is a set of platform-as-a-service products that use OS-level virtualization to deliver software in packages called containers. This product allows you to run and configure Graylog in concert with its dependencies: MongoDB and Elasticsearch or OpenSearch.

**Requirements**
You will need a recent version of Docker, at least v20.10.10. In addition, use the following Docker images in this chapter:

Graylog: graylog/graylog
MongoDB: mongo
OpenSearch: https://hub.docker.com/r/opensearchproject/opensearch
Elasticsearch: https://www.docker.elastic.co/r/elasticsearch

If you prefer to run the containers together, you could configure all the containers above in a yaml file. To do this install Docker Compose. Refer to Settings for compose examples.
