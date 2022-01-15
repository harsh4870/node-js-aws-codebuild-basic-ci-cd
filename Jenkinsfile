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
    stage('Deploy to K8s') {
      steps{
        script {
          sh "sed -i 's,TEST_IMAGE_NAME,harshmanvar/node-web-app:$BUILD_NUMBER,' deployment.yaml"
          sh "cat deployment.yaml"
          sh "kubectl --kubeconfig=/home/ec2-user/config get pods"
          sh "kubectl --kubeconfig=/home/ec2-user/config apply -f deployment.yaml"
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
