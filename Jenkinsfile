pipeline {
    agent any 

     options {
        timeout(time: 10, unit: 'MINUTES')
     }
    environment {
    ACR_NAME = "akeemacr"
    registryUrl = "akeemacr.azurecr.io"
    IMAGE_NAME = "nodejswebapp"
    IMAGE_TAG = "v1.0.0"
    registryCredential  = "akeem-acr"
    }
    stages { 
        stage('SCM Checkout') {
            steps{
           git branch: 'master', url: 'https://github.com/AKEEMABOLORE/nodejs-webapp.git'
            }
        }
        // run sonarqube test
        // stage('Run Sonarqube') {
          // environment {
              //  scannerHome = tool 'ibt-sonarqube';
          // }
           // steps {
             // withSonarQubeEnv(credentialsId: 'ibt-sonar', installationName: 'IBT sonarqube') {
               // sh "${scannerHome}/bin/sonar-scanner"
             // }
           // }
       // }
       // Building Docker Image 
       stage ('Build Docker image') {
        steps {
                script {
                    //dockerImage = docker.build registryUrl
                 def dockerImage = docker.build("${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${IMAGE_TAG}", '.') 
                }
            }
       } 
    // Uploading Docker images into ACR
        stage('Upload Image to ACR') {
         steps{   
             script {
                 docker.withRegistry( "http://${ACR_NAME}.azurecr.io", registryCredential ) {
               // dockerImage.push()
              sh " docker push ${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${IMAGE_TAG}"
                  }
              }
         }
      }
    }
}
