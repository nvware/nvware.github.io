# Docker

- https://dataedo.com/docs/installing-docker-on-windows-via-wsl.
  
# **Installing Docker on Windows via WSL**
1. Install and config WSL
First, we'll need to set up the Windows Subsystem for Linux. To do this, open the command line as administrator. Run these commands to install WSL:
```
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
```
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
After the operation finishes, restart Windows. Then, open command line as administrator, and run this command:
```
  wsl --set-default-version 2
```
2. Install Ubuntu 22 LTS
Next, we'll need to install an actual Linux instance. We'll use Ubuntu in the following steps. Open this link and install Ubuntu via MS Store.

You should see a new window with Linux open. If you don't, restart Windows and retry the Ubuntu installation.

Set up the main user and password in the window that opens. Download and install terminal for future ease of use.

3. Install prerequisites for Docker
Open the terminal and open a new tab for Ubuntu. Run the commands below:
```
    sudo apt update && sudo apt upgrade
```
```
    sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
```   
Still in the terminal, change the network config so that Docker can interact with the firewall using the command below:
```
    update-alternatives --config iptables.
```
When asked, choose iptables-legacy.

4. Install Docker
To avoid issues during the future steps, make sure Ubuntu trusts the Docker packages:
```
    . /etc/os-release
```
```
    curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc
```
```
    echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
```
```
    sudo apt update
```
Now, we can install Docker:
```
    sudo apt install docker-ce docker-ce-cli containerd.io
```
5. Configure our user to work with Docker
Add our user to the Docker group:
```
    sudo usermod -aG docker $USER
```
To confirm the change worked, close the terminal tab and open a new Ubuntu tab, then run:
```
    groups
```
It should now list the docker group. If it doesn't, repeat the previous command.

Now, we'll need to assign a group ID to Docker. First, let's check if an example ID is unused:
```
    getent group | grep 36257 || echo "This ID is not in use."
```
If not, retry with another ID. After finding an unused ID, use it in the following step:
```
    sudo sed -i -e 's/^\(docker:x\):[^:]\+/\1:36257/' /etc/group
```
We'll need to restart Ubuntu now. To do this, close the terminal, then open the command line as administrator and run:
```
    wsl --shutdown
```
After that, go back to the Ubuntu terminal and run:
```
    DOCKER_DIR=/mnt/wsl/shared-docker
    mkdir -pm o=,ug=rwx "$DOCKER_DIR"
    chgrp docker "$DOCKER_DIR"
    sudo mkdir /etc/docker/
```
Open the nano text editor:
```
    sudo nano /etc/docker/daemon.json
```
Paste the following at the end of the file:
```
    {
      "hosts": ["unix:///mnt/wsl/shared-docker/docker.sock"]
    }
```
Press ctrl-s to save, then ctrl-x to close nano. Start Docker:
```
    sudo dockerd
```
If it doesn't work, it should display the PID of the previous process. Use it in the command below:
```
    sudo kill PID
    sudo dockerd
```
Open a new terminal tab and test the config:
```
    export DOCKER_HOST="unix:///mnt/wsl/shared-docker/docker.sock"
    docker run --rm hello-world
```
6. Set Docker to autostart
Let's set up Docker to autostart on WSL start. Open the nano text editor:
```
    sudo nano .bashrc
```
Paste the lines below at the end of the file:
```
    #Docker autostart
    DOCKER_DISTRO="Ubuntu-22.04"
    DOCKER_DIR=/mnt/wsl/shared-docker
    DOCKER_SOCK="$DOCKER_DIR/docker.sock"
    export DOCKER_HOST="unix://$DOCKER_SOCK"
    if [ ! -S "$DOCKER_SOCK" ]; then
        mkdir -pm o=,ug=rwx "$DOCKER_DIR"
        chgrp docker "$DOCKER_DIR"
        /mnt/c/Windows/System32/wsl.exe -d $DOCKER_DISTRO sh -c "nohup sudo -b dockerd < /dev/null > $DOCKER_DIR/dockerd.log 2>&1"
    fi
```
Press ctrl-s to save, then ctrl-x to close nano.

Next, we'll add WSL to autostart to run Docker whenever Windows restarts. Open Windows Scheduler, then add a task on startup with action:
```
    "C:\Windows\System32\wsl.exe" -d Ubuntu-22.04
```
7. (Optional) Allow passwordless access to Docker
If when opening terminal, it starts to prompt you for the password each time, run:
```
    sudo visudo
```

Paste the lines below at the end of the file:

```
    #allow passwordless access to Docker
    %docker ALL=(ALL)  NOPASSWD: /usr/bin/dockerd
```

Press ctrl-s to save, then ctrl-x to close nano.

8. Add the Dataedo Portal docker image
Finally, to add the Dataedo Portal docker image, follow this guide.
