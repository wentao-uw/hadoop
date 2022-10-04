## 环境准备
1. 3 aws EC2 instances
2. windows 10
3. vscode


## ssh
1. 把 `hadoop.pem` 放到 C:\Users\Wentao\.ssh 
2. 启动aws instance，获取你的Public IPv4 address,每次重启instance后ip会发生改变所以每次连接时需要修改config文件
3. 修改 config 文件，它在C:\Users\Wentao\.ssh 
    ```shell
    Host hadoop
        HostName [Public IPv4 address,具体填写的时候不需要加[]]
        User ec2-user
        Port 22
        IdentityFile C:\Users\Wentao\.ssh\hadoop.pem
    ```
## install docker on ubuntu
1. https://docs.docker.com/engine/install/ubuntu/
```shell
# Set up the repository
 sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker’s official GPG key:
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Use the following command to set up the repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
2. Manage Docker as a non-root user
- https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user
```shell
# Create the docker group.
sudo groupadd docker
# Add your user to the docker group.
sudo usermod -aG docker $USER
# group membership is re-evaluated
newgrp docker 
# Verify that you can run docker commands without sudo
docker run hello-world
```
3. Configure Docker to start on boot
   - ubuntu 不需要设置
   - https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot
4. `docker pull ubuntu`
5. check downloaded images: `docker images`
6. `mkdir build`
7. Install Java 8 on Ubuntu: `sudo apt install openjdk-8-jdk`
8. `cd /usr/lib/jvm`
9. `cp -r java-1.8.0-openjdk-amd64/ ~/build/`
10. `cd ~/build/`
11. `docker run -it -v /home/ubuntu/build/:/root/build --name ubuntu ubuntu`
12. `cd root/build/`
13. `apt-get install nano`
14. `apt-get install ssh`
15. `apt-get install net-tools`
16. `/etc/init.d/ssh start`
18. `nano ~/.bashrc`
19. “Alt+ /” nano editor go to end of line
20. add `/etc/init.d/ssh start`
21. `source ~/.bashrc`
22. `ssh-keygen -t rsa`, 所有需要输入的地方都回车跳过
23. `cd /root/.ssh`, 检查是否有id_rsa
24. `cat id_rsa.pub >> authorized_keys`
25. `ssh localhost`,连接成功
26. `logout`
27. `apt-get install default-jdk`
28. `cd /usr/lib/jvm`
29. `mv /root/build/java-1.8.0-openjdk-amd64 /usr/lib/jvm`
30. `nano ~/.bashrc`
31. add `export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/`
`export PATH=$PATH:$JAVA_HOME/bin`
32. `source ~/.bashrc`
33. `java -version`
34. 打开新的ssh
35. `docker ps`
36. 找到 image = ubuntu， command = “bash”的container
37. `docker commit 67f4c5266081 ubuntu/jdkinstalled`
38. `docker images`, 找到ubuntu/jdkinstalled
39. `docker run -it -v /home/ubuntu/build/:/root/build --name ubuntu-jdkinstalled ubuntu/jdkinstalled`
40. download hadoop
    1.  https://hadoop.apache.org/releases.html
    2.  use binary file
    3.  `wget https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.2.4/hadoop-3.2.4.tar.gz`
41. `mv hadoop-3.2.4.tar.gz /root/build`
42. `tar -zxvf hadoop-3.2.4.tar.gz -C /usr/local`
43. `cd /usr/local`
44. `mv hadoop-3.2.4/ hadoop`
45. `cd hadoop/`
46. `./bin/hadoop version` get hadoop version
47. `cd etc/hadoop`
48. 修改5个config file，复制粘贴doc文档中的设置
49. 打开另外一个终端，`docker ps`
50. `docker commit fbc218db6159 ubuntu/hadoopinstalled`
51. 分别打开三个终端，输入：`docker run -it -h master --name master ubuntu/hadoopinstalled`, `docker run -it -h slave01 --name slave01 ubuntu/hadoopinstalled`, `docker run -it -h slave02 --name slave02 ubuntu/hadoopinstalled`



## ref
1. https://www.bilibili.com/video/BV1v34y1v7JE/?spm_id_from=333.337.search-card.all.click&vd_source=31e41ebdb861c49989aba6aed21914f7