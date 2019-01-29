pipeline {
  agent none
  stages {
    stage('Build') {
      parallel {
        stage('Build-Client') {
          agent any
          steps {
            echo 'This is branch client'
            bat '%cd%/client/jenkins/scripts/build.bat'
          }
        }
        stage('Build-Server') {
          agent any
          steps {
            echo 'This is branch server'
            bat '%cd%/server/jenkins/scripts/build.bat'
          }
        }
        stage('test') {
          steps {
            dir(path: 'client') {
              bat 'npm install'
            }

          }
        }
      }
    }
    stage('Test') {
      parallel {
        stage('Test-Client') {
          agent any
          steps {
            echo 'This is branch client'
            bat '%cd%/client/jenkins/scripts/test.bat'
            dir(path: './client') {
              bat 'npm test'
            }

          }
        }
        stage('Test-Server') {
          agent any
          steps {
            echo 'This is branch server'
            bat '%cd%/server/jenkins/scripts/test.bat'
          }
        }
      }
    }
    stage('Deliver') {
      steps {
        bat '%cd%/client/jenkins/scripts/delivery.bat'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        bat '%cd%/client/jenkins/scripts/kill.sh'
      }
    }
  }
  environment {
    CI = 'true'
  }
}