pipeline {
    agent any
    environment{

        dockerImage = ''

        registryurl = 'muralidhar123/dbmysql'

        dockercred = 'dockerhub' 
    }

    stages {
        stage("scm using git"){
        
            steps {
                git url: 'https://github.com/satyamuralidhar/mysql-docker-pipeline.git'
            }
        }
           
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registryurl + ":v$TAG"
                }
            }
        }
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', dockercred ) {
                        dockerImage.push()
                    }   
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registryurl:v$TAG"
            }
        }
    }
       
    // slack notification  
    post {
        success {
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.TAG}]' (${env.BUILD_URL})")

        }
        failure {
            slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.TAG}]' (${env.BUILD_URL})")
        } 
    }        
         
} 
