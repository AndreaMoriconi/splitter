pipeline {
  environment {
    registry = "andreamoriconi/splitter1"
    registryCredential = 'andreamoriconi'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/AndreaMoriconi/splitter.git'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('compile') {
      steps {
        sh 'npm run build'
      }
    }
    stage('Building image') {
      steps{
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
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}