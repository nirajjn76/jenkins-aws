2 ec2 instance
1st instance.

1- install jenkins- jenkins (master node) job which will clone code from git create docker image of that java app, push that image to the docker hub.
2- 2nd jenkins job- copy whole code to 2nd instance.

in 2nd instance the java app should run. published.
tomcat server




1. spin  up 2 instance
2. install jenkins on 1st instance.
3. install tomcat on second instance.
4. create a pipeline job on 1st instance which copies code and builds the project and builds image and pushes it to dockerhub.
5. create a 2nd pipeline job.
6. i used tomcat7 maven plugin to deploy .war file to 2nd instance's tomcat server url. add it to pom.xml



<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <url>http://3.108.67.98:8080/manager/text</url>
        <server>TomcatServer</server>
        <path>/webapp</path>
    </configuration>
</plugin>


mvn tomcat7:deploy

this command will publish the war to given address of tomcat server.
