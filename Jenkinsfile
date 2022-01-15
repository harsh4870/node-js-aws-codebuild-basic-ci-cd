pipeline {
  environment {
    imagename = "harshmanvar/node-web-app"
    registryCredential = 'docker'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/harsh4870/node-js-aws-cloudbuild-basic-ci-cd.git', branch: 'main', credentialsId: 'github'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Apply Kubernetes files') {
    withKubeConfig([credentialsId: '3f5bcb63-2824-41e6-aed9-084d2248646a', serverUrl: 'https://808A4C0F41A833A1EA96030AE2DE77DE.gr7.us-west-2.eks.amazonaws.com']) {
      sh 'kubectl get pods'
    }
  }
    
    
  }
}
