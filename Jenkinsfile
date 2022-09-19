pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/amdadul1994/nodeapps-automation']]])
      }
    }

    stage('docker build'){
        steps{
        script{
            sh 'docker build -t himu1994/node-apps-automation .'
        }
        }
    }

    stage('docker hub push'){
        steps{
        script{
            withCredentials([string(credentialsId: 'docker-secret', variable: 'dockerlogin')]) {
            sh 'docker login -u himu1994 -p ${dockerlogin}'  
        } 
            sh 'docker push himu1994/node-apps-automation'
        }
        }
    }

    stage('docker to k8s'){
        steps{
        script{
            kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpwd')
        }
        }
    }     

  }

}