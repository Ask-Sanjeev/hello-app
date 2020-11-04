pipeline {
    
    environment {
    registry = "sanjeevk2020/hello-app-tomcat-new"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

agent any

tools {
    maven 'maven'
    jdk 'java'
}

stages {
    
    stage ("Cloning Git") {
        steps {
            git  'https://github.com/Ask-Sanjeev/hello-app.git'
            }
        }

    stage ("Build Image") {
    
      steps {
          script{
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }
          }
        }
        
    stage ("Docker Deploy") {
    
      steps { 
          script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
      
      stage ("Slack notification") {
          steps {
              slackSend (channel: 'alerts', message: "deployment is successful, here is the info - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
          }
      }
 }
 }
