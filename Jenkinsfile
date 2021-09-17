pipeline 
{
    agent any
    


    stages 
    {
        stage('SCM Checkout')
        {
           steps 
           {
              
                 git branch: 'master', credentialsId: 'GITLAB_CRED', url: 'https://gitlab.com/siddharth.alhat/git-docker-jenkins-task-AWS.git'
            }
        }


   stage('Maven Package')
        {
          steps
            {
              script
                {
                   sh "mvn clean package install"
                }
            } 
        }

       

            }
        }

     


