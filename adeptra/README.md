===============

This is the simplest possible Java webapp for testing servlet container deployments.  It should work on any container and requires no other dependencies or configuration.


##############Installing Docker 
###############################################################################################################

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update

apt-cache policy docker-ce

#Output of apt-cache policy docker-ce
#docker-ce:
#  Installed: (none)
#  Candidate: 18.06.1~ce~3-0~ubuntu

sudo apt-get install -y docker-ce

sudo systemctl status docker

#The output should be similar to the following, showing that the service is active and running:

#Output
#● docker.service - Docker Application Container Engine
#   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
#  Active: active (running) since Thu 2018-10-18 20:28:23 UTC; 35s ago

###############################################################################################################


##############Installing Jenkins 
############################################################################################################

Step 1: Install Java and set path

sudo add-apt-repository ppa:webupd8team/java

sudo apt update; sudo apt install oracle-java8-installer

javac -version

sudo apt install oracle-java8-set-default

export JAVA_HOME=/usr/lib/jvm/java-8-oracle/

PATH=$PATH:JAVA_HOME


Step 2: Installing Jenkins on EC2

 First, we’ll add the repository key to the system, run the below command from your terminal

wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -

When the key is added, the system will return OK.

Next, we’ll append the Debian package repository address to the server’s sources.list:

echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list

sudo apt-get update

sudo apt-get install jenkins -y

sudo systemctl start jenkins

Verify that Jenkins has started 

sudo systemctl status jenkins



Step 3 Configuring the Firewall Settings

Jenkins runs on port 8080 by default, allow access to the port

sudo ufw allow 8080

sudo ufw status

If you receive an inactive status after running the above command, run the below command to fix it


###following not needed
sudo ufw enable

sudo cat /var/lib/jenkins/secrets/initialAdminPassword



Step 4 Setting up Jenkins

Because our Jenkins server is on AWS we will need to expose port :8080 in our security group

Jump back into the EC2 console view, select Security Groups from the left side scroll bar

############################################################################################################


##### https://www.howtoforge.com/tutorial/ubuntu-jenkins-master-slave/
####################################################################################
####on slave

###install java as above for master an create user Jenkins and set password

useradd -m -s /bin/bash Jenkins

passwd Jenkins

####################################################################################

###set PasswordAuthentication yes in /etc/ssh/sshd_config
####################################################################################

sudo /etc/ssh/sshd_config /etc/ssh/sshd_config_backup

sudo vi /etc/ssh/sshd_config

#restart service

service sshd restart

####################################################################################

###on master
####################################################################################

ssh-keygen

ssh-copy-id Jenkins@EC2_SLAVE_IP   ####change EC2_SLAVE_IP to your slave ip

###try login it shouldn't ask for password

####use non verifying key strategy while creating node

####################################################################################



https://code.cognizant.com

https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7)


https://linuxize.com/post/how-to-install-and-use-docker-on-centos-7/
https://linoxide.com/linux-how-to/install-jenkins-docker-container/
https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+with+Docker

https://unix.stackexchange.com/questions/185365/mail-cannot-send-message-process-exited-with-a-non-zero-status
velexy.teck



