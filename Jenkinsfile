pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        sh 'echo Git Checkout from the Repository'
        git(url: 'https://github.com/felipecosta09/java-goof.git', branch: 'master', poll: true)
      }
    }

    stage('Source Code Test') {
      steps {
        sh 'echo Source Code Test made by Snyk...'
      }
    }

    stage('Container Build') {
      steps {
        sh '''echo Build the Docker Container
echo 
docker build -t java-app:latest .'''
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''echo Logging in to Amazon ECR...
aws --version
$(aws ecr get-login --no-include-email --region us-east-1)
echo 
echo Tagging Docker Build to prepare to Push
docker tag java-app:latest 650143975734.dkr.ecr.us-east-1.amazonaws.com/java-app
echo 
echo Push the New Image to ECR
docker push 650143975734.dkr.ecr.us-east-1.amazonaws.com/java-app'''
      }
    }

    stage('Cloud One Container Image Scan') {
      steps {
        sh 'echo Container Image Scan from CLoud One'
      }
    }

    stage('Dev Tests') {
      parallel {
        stage('Dev Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Unit Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Deploy Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        sh 'echo Deploy New Container to Fargate'
      }
    }

  }
}