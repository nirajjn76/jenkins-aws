pipeline { 

    environment { 

        registry = "nirajjn76/jenkins-aws" 

        registryCredential = 'docker' 

        dockerImage = '' 

    }

    agent any 

    stages { 
        stage('Cloning') { 

            steps { 

                git 'https://github.com/nirajjn76/jenkins-aws' 

            }

        } 
        
        
        stage('Maven Package')
        {
          steps
            {
                
                   sh "mvn clean package -DskipTests"
                
            } 
        }

        stage('Build image') { 

            steps { 

                script { 

                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                }

            } 

        }

        stage('Push image') { 

            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 

                        dockerImage.push() 
                    }

                } 

            }

        } 

       // stage('delete latest image') { 

         //   steps { 

           //     sh "docker rmi $registry:$BUILD_NUMBER" 

        //    }

    //    } 

    }

}
