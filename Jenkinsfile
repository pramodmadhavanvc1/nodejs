node{
    agent any
    tools {
      maven 'M2_HOME'
    }
    environment {
      DOCKER_TAG = getVersion()
    }

    stages {
        stage('SCM') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/pramodmadhavanvc1/jenkins-docker-example.git']]])

             
            }
        }
       stage('Maven Build'){
           steps{
               sh "mvn clean package"
           }
       }
       stage('Docker Build'){
           steps{
               sh "docker build . -t pramodmadhavan/nodejsapp-1.0:${DOCKER_TAG} " 
           }
       }
       stage('Dockerhub Push'){
           steps{
               withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                   sh "docker login -u pramodmadhavan -p ${dockerHubPwd}"
               }
               
               sh "docker push pramodmadhavan/nodejsapp-1.0:${DOCKER_TAG} " 
           }
       }   
    }
}
def getVersion(){
    def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
