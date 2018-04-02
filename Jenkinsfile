#!groovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t mnallathiga/spring-petclinic:latest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'mkDockerHub', passwordVariable: 'mkDockerHubPassword', usernameVariable: 'mkDockerHubUser')]) {
          sh "docker login -u ${env.mkDockerHubUser} -p ${env.mkDockerHubPassword}"
          sh 'docker push mnallathiga/spring-petclinic:latest'
        }
      }
    }
    post {
        always {
            archive "target/**/*"
            junit 'target/surefire-reports/*.xml'
        }
    }
  }
}
