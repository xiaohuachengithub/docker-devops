pipeline {
  environment {
    registry = "xiaohuachengithub/trial"
    registryCredential = 'dockerhubcred'
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        git url: 'https://github.com/xiaohuachengithub/docker-devops'  
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
      }
        }
      }
      stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('kubernetes runs') {
      steps{
        sh 'sed -i "s/BUILDNUMBER/$BUILD_NUMBER/" dep.yml'   
        sh 'kubectl apply -f dep.yml || echo "nothing to create"' 
      }
    }
    }
  }
