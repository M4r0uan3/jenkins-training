pipeline {
  agent any
  tools {
    maven 'Maven3'
  }
   options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker-creds')
  }
  stages {
    stage('Build and Test') {
            steps {
              sh 'chmod +x ./mvnw'
              sh 'mvn clean package'
            }
        }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t na9ani9/k8s-spring-boot-test:latest .'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push na9ani9/k8s-spring-boot-test:latest'
      }
    }
    // stage('Deploying App to Kubernetes') {
    //   steps {
    //     script {
    //       withKubeConfig(clusterName: 'kubernetes', credentialsId: 'jenkins-sa', namespace: 'jenkins', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.56.11:6443') {
    //         sh 'kubectl apply -f deployment.yaml'
    //       }
    //     }
    //   }
    // }

  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

