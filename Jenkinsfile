currentBuild.displayName = "vprofile-v1-#"+currentBuild.number
pipeline{
    agent any 
    stages{
        stage("Source-Code"){
            steps{
                //git credentialsId: '194a61eb-0d84-4505-b0f8-08a1fc4ce10f', url: 'https://github.com/krishna1083/KrishnaVprofile.git'
            
                git url: 'https://github.com/krishna1083/test.git'
            }
        }
        stage("Maven-build"){
            steps{
                sh "mvn clean install"
            }
        }
       
            
        }
    }
