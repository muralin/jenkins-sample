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
        sh 'docker build -t muralin/spring-petclinic:latest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'mkDockerHub', passwordVariable: 'mkDockerHubPassword', usernameVariable: 'mkDockerHubUser')]) {
          sh "docker login -u ${env.mkDockerHubUser} -p ${env.mkDockerHubPassword}"
          sh 'docker push muralin/spring-petclinic:latest'
        }
      }
    }
  }
}
