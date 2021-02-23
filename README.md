# expose-the-docker-API-over-TCP



Navigate to /lib/systemd/system in your terminal and open docker.service file
vi /lib/systemd/system/docker.service

Find the line which starts with ExecStart and adds -H=tcp://0.0.0.0:2375 to make it look like
ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375
Save the Modified File
Reload the docker daemon
systemctl daemon-reload
Restart the container
sudo service docker restart
Test if it is working by using this command, if everything is fine below command should return a JSON
curl http://localhost:2375/images/json
To test remotely, use the PC name or IP address of Docker Host

 
https://aws.amazon.com/ru/getting-started/hands-on/ec2-auto-scaling-spot-instances/

user-data

#!/bin/bash

apt update
apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt update
apt-cache policy docker-ce
apt install docker-ce -y
systemctl start docker

var1="ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock"
var2="ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375"
sed -i -e "s~$var1~$var2~g" /lib/systemd/system/docker.service

systemctl daemon-reload
/etc/init.d/docker restart


Create Template for docker spot instance 

Create SG for ELB & instance

Create domain docker.k8s.***.com for  instance

docker.k8s.***.com CNAME Clasic LoadBalancer

Create scaling policies in Auto Scaling groups 

and testing 

docker -H docker.k8s.***.com:2375 build -t test1 .
 

~/Downloads/***/git/dynamiq-cms-nodejs on  master! ⌚ 15:34:29
$ docker -H docker.k8s.***.com:2375 images          
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
test1        latest    27238058e4c0   4 minutes ago   1.99GB
test         latest    27238058e4c0   4 minutes ago   1.99GB
node         8         8eeadf3757f4   14 months ago   901MB

