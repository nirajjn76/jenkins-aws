# maven-project

Simple Maven Project

----------------How I acheived the output---------------

**Create 2 EC2 Instances**

1st Instance

Step-1 : Setup Jenkins

echo "update"
apt-get update -y

echo "jdk"
apt-get install openjdk-8-jdk -y

echo "Check version"
java -version

echo "java"
readlink -f $(which java)
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
echo $JAVA_HOME
export JAVA_HOME
PATH=$PATH:$JAVA_HOME

echo "wget"
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

echo "address of file"
 sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
    
echo "update"
apt-get update -y

echo "Jenkins Install"
 sudo apt-get install jenkins -y

echo "start jenkins"
service jenkins start

echo "enable jenkins"
systemctl enable jenkins

Step-2 : Configure Jenkins

-> Install the plugins. (Git, Credentials Binding Plugin, Docker plugin, docker-build-step )
 Create two Pipeline-Jobs  In pipeline section write code. I choose to take code from SCM and after that mention the jenkinsfile name (Mostly the file name is Jenkinsfile)

 First Job will pull code from Git -> build it using Maven -> build Docker image out of it -> push to DockerHub
 Second Job Will send the war file (generated by maven) to second Instance

#My Script for First Jenkinsfile : https://gitlab.com/siddharth.alhat/git-docker-jenkins-task-AWS/-/blob/master/Jenkinsfile-AWS

#My Script for Second Jenkinsfile : 
	
	pipeline
	{
	agent any 
	triggers {
			upstream(upstreamProjects: 'simple-project-1', 			
			threshold: hudson.model.Result.SUCCESS)//UNSTABLE, FAILURE, NOT_BUILT, ABORTED
		}	
	stages
	{
		stage("Send to Instance-2"){
				steps{
				sshagent(['deploy_user']) {
					sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/hello/webapp/target/webapp.war ubuntu@15.206.127.233:/opt/apache-tomcat-8.5.71/webapps"
					
					}
				}

		
	}
	}
	}



2nd Instance

Step-1 : Setup Tomcat

#update adn Install Java
apt-get update
apt-get install openjdk-8-jdk -y

#Change Directory
cd /opt

#Download and Extract
wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.71/bin/apache-tomcat-8.5.71.tar.gz
tar -xvzf apache-tomcat-8.5.71.tar.gz 

#CHanging the Permissions
chmod +x apache-tomcat-8.5.71/

#Creating a Softlink 
ln -s /opt/apache-tomcat-8.5.71/bin/startup.sh /usr/local/bin/tomcatup
ln -s /opt/apache-tomcat-8.5.71/bin/shutdown.sh /usr/local/bin/tomcatdown

#Search and delete the line starting with value
find / -name  tomcat-users.xml
above command gives 3 context.xml files. comment () Value ClassName field on files which are under webapp directory. After that restart tomcat services to effect these changes

#Update users information in the tomcat-users.xml file goto tomcat home directory and Add below users to conf/tomcat-user.xml file

	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="tomcat" roles="manager-gui"/>
Restart serivce and try to login to tomcat application from the browser. This time it should be Successful

after scp command it u get error like permission denied the give user chown permission for /opt directory only

chown -R ubuntu:ubuntu /opt









