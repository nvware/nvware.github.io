# Installing Graylog on Docker in WSL (Ubuntu)
https://go2docs.graylog.org/5-0/downloading_and_installing_graylog/docker_installation.htm?tocpath=Downloading%20and%20Installing%20Graylog%7CInstalling%20Graylog%7C_____2

**Docker**
Docker is a set of platform-as-a-service products that use OS-level virtualization to deliver software in packages called containers. This product allows you to run and configure Graylog in concert with its dependencies: MongoDB and Elasticsearch or OpenSearch.

**Requirements**
You will need a recent version of Docker, at least v20.10.10. In addition, use the following Docker images in this chapter:

- **Graylog**: graylog/graylog

- **MongoDB**: mongo

- **OpenSearch**: https://hub.docker.com/r/opensearchproject/opensearch

- **Elasticsearch**: https://www.docker.elastic.co/r/elasticsearch


If you prefer to run the containers together, you could configure all the containers above in a yaml file. To do this install Docker Compose. Refer to Settings for compose examples.

Installing Graylog on Docker in WSL (Ubuntu)
1. **Update Package Index:**
  sudo apt update && sudo apt upgrade

2. **Install Dependencies:**
  sudo apt install apt-transport-https ca-certificates curl software-properties-common

3. **Add Docker GPG Key:**
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

4. **Add Docker Repository:**
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

5. **Install Docker:**
  sudo apt update
  sudo apt install docker-ce

6. **Start Docker Service:**
  sudo systemctl start docker
  sudo systemctl enable docker

7. **Pull Graylog Docker Image:**
  docker pull graylog/graylog:5.0

8. **Run Graylog Container:**
  docker run --link mongo --link elasticsearch -p 9000:9000 -p 12201:12201 -p 1514:1514 -p 5555:5555 -e GRAYLOG_HTTP_EXTERNAL_URI="http://127.0.0.1:9000/" -d graylog/graylog:5.0

9. **Configure Graylog Inputs:**
- Navigate to your Graylog port (e.g., localhost:9000/system/inputs).
- Create a Raw/Plaintext TCP input.
- Provide a name for the input and select the node (or choose “Global”).
- Send a plain text message to the Graylog Raw/Plaintext TCP input running on port 5555:
    echo 'First log message' | nc localhost 5555

10. **Access Graylog UI:**
- Open your web browser and go to http://127.0.0.1:9000/.
- Set the admin user password via environment variable:
        -e GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918

Remember to adjust the configuration if you plan to run Graylog on external servers. For security settings, consult the Graylog documentation
