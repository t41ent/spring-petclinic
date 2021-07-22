pipeline {
  environment {
    registry = "t41ent/petclinic"
    registryCredential = 'docker_hub'
    dockerImage = ''
    DOCKER_IMAGE_NAME = "t41ent/petclinic"
  }
  agent { label 'jenkins-docker' }
  tools {
    maven 'Maven 3.8.1'
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
    stage('Build app') {
      steps {
      sh 'mvn install -DskipTests'
      }
    }
    stage('Building Image') {
      steps{
        container ('docker') { 
          script {
            dockerImage = docker.build registry + ":latest"
          }
        }
      }   
    }
      stage('Deploy Image to registry') {
        steps{
          container ('docker') {
            script {
                docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                }
            }
          }
        }
      }
    
    stage('Deploy to production'){
      steps{
      input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    configs: 'petclinic.yaml',
                    enableConfigSubstitution: true)
      }
    }
  }
}
