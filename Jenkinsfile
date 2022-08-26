pipeline {
  environment {
    imagename = "pramodh156/maven5"
    registryCredential = 'pramodh156-dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/Pramodh156/new_proj.git', branch: 'main', credentialsId: 'Pramodh156-github-user-token'])

      }
    }
    stage('Building Custom Image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Custom Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Docker Deploy to Tomcat') {
     steps{
         script {
            dockerImage.run("-p 8097:8080 --rm --name pramodh10")
         }
     }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}   
