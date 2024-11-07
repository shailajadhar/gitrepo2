pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/shailajadhar/gitrepo2', branch: 'main', credentialsId: 'b0bd8c94-b6dc-4359-a885-604a8964fb3a')
      }
    }

    stage('Build&Publish') {
      steps {
        sh '''sudo docker build -t shailajadhar/srepo1:latest .
sudo docker push shailajadhar/srepo1:latest
'''
      }
    }

    stage('Deploy') {
      parallel {
        stage('DeployDev') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 7777:80 --name devcon1 shailajadhar/srepo1:latest'''
          }
        }

        stage('DeployQA') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 9999:80 --name QAcon1 shailajadhar/srepo1:latest'''
          }
        }

      }
    }

  }
}