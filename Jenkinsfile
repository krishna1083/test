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
        stage("Nexus-Repo-Upload"){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'vprofile-v1', classifier: '',
                                                 file: 'target/vprofile-v1.war',
                                                 type: 'war']],
                                                 credentialsId: '49a4a510-e08e-4f8e-baf3-5c6563c8a25a',
                                                 groupId: 'vprofile-v1',
                                                 nexusUrl: '172.31.43.32:8081//nexus/content/repositories/krishna',
                                                 nexusVersion: 'nexus2',
                                                 protocol: 'http', 
                                                 repository: 'krishna', 
                                                 version: '$BUILD_ID'
            }
        }
    
        stage("SonarQube"){
            steps{
                withSonarQubeEnv('sonarqube'){
                sh "mvn sonar:sonar"
            }
        }
        }
    
        stage("Tal-Deploy"){
            steps{
                sshagent(['deploy_user']) {

    // scp <src_file> username@IP:<dest_path>>

                    sh "scp -o StrictHostKeyChecking=no target/vprofile-v1.war ubuntu@172.31.42.74:/opt/apache/webapps" 
                    
}
            }
            }
        
        stage("TAL-Build-Status-Natification"){
            steps{
                slackSend channel: 'pipe-lines', message: 'Tal-Deployment was Successful'
            }
        }    
            
        stage("Dev-Evn-Deploy-Approval"){
            steps{
                echo "Taking approval from Dev manager for Dev deploy"
                timeout(time: 7, unit: 'DAYS') {
                input message: "Do you want to deploy in to Dev?", submitter: 'admin'
                
                }
            }
        }    
          
        stage("Dev-Build-Status-Natification"){
            steps{
                slackSend channel: 'pipe-lines', message: 'TAL-Deployment was Successful, Please start your testing'
            }
        }     
            
        }
    }
