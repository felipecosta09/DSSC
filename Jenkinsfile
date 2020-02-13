pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      parallel {
        stage('Git Checkout') {
          steps {
            git(url: 'https://github.com/felipecosta09/DSSC.git', branch: 'master', poll: true)
          }
        }

        stage('Static Code Analysis') {
          steps {
            sleep 5
          }
        }

      }
    }

    stage('Container Build') {
      steps {
        sh 'docker build -t web-app:latest .'
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''aws --version
$(aws ecr get-login --no-include-email --region us-east-1)
docker tag web-app:latest 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app
docker push 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app'''
      }
    }

    stage('Cloud One Container Image Scan') {
      steps {
        smartcheckScan(imageName: 'web-app', smartcheckHost: '1.1.1.1', smartcheckCredentialsId: 'dssc-credentials', findingsThreshold: '{"malware":0,"vulnerabilities":{"defcon1":0,"critical":0,"high":0},"contents":{"defcon1":0,"critical":0,"high":0},"checklists":{"defcon1":0,"critical":0,"high":0}}', insecureSkipRegistryTLSVerify: true, insecureSkipTLSVerify: true)
      }
    }

    stage('Dev Tests') {
      parallel {
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

        stage('Manual Test') {
          steps {
            input 'Approve?'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy New Container to Fargate'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(channel: '#aws-account-alerts', color: 'good', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}")
      }
    }

  }
}
