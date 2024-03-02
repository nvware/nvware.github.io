- **Installing mysql** on Docker
- To connect our apps to MySQL running on Docker Desktop on Windows or wsl

# docker

# **Install mysql**

1-Docker Pull Command :
- c>docker pull mysql/mysql-server

2-create container mysql01
- c>docker run --rm -d --name mysql01 -e MYSQL_ROOT_PASSWORD=root  -p 33306:3306 mysql/mysql-server:latest

3-connect to mysql db : 
c>docker exec -it mysql01 mysql -uroot -proot

- mysql>select user,host from mysql.user;

- mysql>CREATE USER IF NOT EXISTS root@'%' IDENTIFIED BY 'root';
- mysql>SET PASSWORD FOR root@'%' = PASSWORD('root');
- mysql>GRANT ALL ON \*.* TO root@'%' WITH GRANT OPTION;
- mysql>SHOW GRANTS FOR root@'%';


4-Check the firewall settings: If you are running a firewall on your host machine, make sure that it is not blocking incoming connections on port 33306. You may need to add a rule to allow incoming connections on this port.

This step refers to checking the firewall settings on the host machine where you are running Docker Desktop. So if you are using Docker Desktop on a Windows machine, then you should check the firewall settings on that machine. If you are running Docker on a different operating system, then you should check the firewall settings for that operating system.

To check the firewall settings on a Windows machine, follow these steps:

- Open the Windows Defender Firewall settings: Press the Windows key + R to open the Run dialog box, then type "control firewall.cpl" and press Enter.

- Check the inbound rules: In the Windows Defender Firewall settings, click on "Advanced settings" in the left pane. Then, click on "Inbound Rules" in the left pane to view the inbound rules.

- Look for a rule for port 33306: Scroll down the list of inbound rules and look for a rule that allows incoming connections on port 33306. If such a rule does not exist, you will need to create one.

- Create a new inbound rule: In the Windows Defender Firewall settings, click on "New Rule" in the right pane. This will open the "New Inbound Rule Wizard."

- Configure the new rule: Follow the steps in the wizard to create a new inbound rule that allows incoming connections on port 3306. You can choose to allow the connection for specific IP addresses or for all IP addresses.

After creating the new inbound rule, you should be able to connect to the MySQL container running in Docker from your host machine without any issues.

