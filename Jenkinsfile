pipeline {
  environment {
    registry = "t41ent/petclinic"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent {kubernetes}
  tools {
    maven 'Maven 3.3.9'
    jdk 'jdk'
  } 
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/t41ent/spring-petclinic.git'
      }
    }
    stage('Compile') {
       steps {
         sh 'mvn compile' //only compilation of the code
       }
    }
    stage('Test') {
      steps {
        sh '''
        mvn clean install
        ls
        pwd
        ''' 
        //if the code is compiled, we test and package it in its distributable format; run IT and store in local repository
      }
    }
    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
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
        sh "docker rmi $registry:latest"
      }
    }
  }
}
