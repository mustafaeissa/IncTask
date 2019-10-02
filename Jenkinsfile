pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/mustafaeissa/Vtask'
      }
    }
    stage('Building & Pushing Imaages') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhup', url: 'https://hub.docker.com') {
          sh 'ansible-playbook docker.yml'
        }
      }
    }
    stage('Test & Build CountriesApp') {
      steps {
        sh 'helm lint ./countries-app'
        sh 'helm install --name IncLAB ./countries-app'
      }
    }
    stage('Test & Build AirportsApp') {
      steps {
        sh 'helm lint ./airports-app'
        sh 'helm install --name IncLAB ./airports-app'
      }
    }
    stage('Create an Ingress Object to Expose Apps') {
      steps {
        sh 'kubectl apply -f ingress-nginx.yaml'
      }
    }
  }
}