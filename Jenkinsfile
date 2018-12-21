pipeline {
  agent none
  stages {
    stage('Building & Unit Testing') {
      parallel {
        stage('Building Server 1') {
          agent {
            label 'server1'
          }
          steps {
            dir(path: 'Server1') {
              sh 'ls'
              checkout scm
              sh 'docker-compose build'
              sh 'pwd'
              sh 'ls'
              sh 'cat test'
            }

          }
        }
        stage('Building Server 2') {
          agent {
            label 'server2'
          }
          steps {
            dir(path: 'Server2') {
              git(branch: 'master', credentialsId: 'git-token', url: 'https://github.com/alexbuicescu/http-hello-world2')
              sh 'docker-compose build'
              sh 'pwd'
              sh 'ls'
            }

          }
        }
      }
    }
    stage('Integration Testing') {
      parallel {
        stage('Starting Server 1') {
          agent {
            label 'server1'
          }
          steps {
            dir(path: 'Server1') {
              sh 'docker-compose up'
            }
          }
        }
        stage('Starting Server 2') {
          agent {
            label 'server2'
          }
          steps {
            dir(path: 'Server2') {
              sh 'docker-compose up'
            }
          }
        }
        stage('Health Checking & Testing') {
          agent none
          stage('Health Checking Server 1') {
            agent {
              label 'server1'
            }
            steps {
              dir(path: 'Server1') {
                sh './health.sh --url=http://localhost:8000'
              }
            }
          }
          stage('Health Checking Server 2') {
            agent {
              label 'server2'
            }
            steps {
              dir(path: 'Server2') {
                sh './health.sh --url=http://localhost:8000'
              }
            }
          }
          stage('Testing') {
            agent {
              label 'server1'
            }
            steps {
              dir(path: 'Server1') {
                sh 'echo "testing..."'
              }
            }
          }
        }
      }
    }
  }
}