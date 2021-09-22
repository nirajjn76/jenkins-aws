pipeline
{
  agent any 
   triggers {
        upstream(upstreamProjects: 'jenkins-docker-aws', 			
        threshold: hudson.model.Result.SUCCESS)//UNSTABLE, FAILURE, NOT_BUILT, ABORTED
    }	
   stages
   {
       //stage("scp to 2nd instance"){
        //    steps{
         //     sshagent(['tomcat']) {
           //     sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/jenkins-docker-aws/webapp/target/webapp.war ubuntu@3.108.67.98:/var/lib/tomcat9/webapps"
             //   //sh "mv /var/lib/tomcat9/webapps/webapp.war web.war" 
               // echo 'click http://ec2-3-108-67-98.ap-south-1.compute.amazonaws.com:8080/webapp'
                //}
            //}

        stage("deploy"){
            steps{
           sh "cd /var/lib/jenkins/workspace/jenkins-docker-aws && mvn tomcat7:deploy"
}
        }     
   }
}

