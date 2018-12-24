pipeline {
  agent none
  stages {
    stage('Building & Unit Testing') {
      parallel {
        stage('Building Server 1') {
          node('server1') {
            dir(path: 'Server1') {
              sh 'ls'
              sh 'echo $USER'
              checkout scm
              sh 'docker-compose build'
              sh 'pwd'
              sh 'ls'
              sh 'cat test'
            }

          }
        }
        stage('Building Server 2') {
          node('server2') {
            dir(path: 'Server2') {
              sh 'echo $USER'
              git(branch: 'master', credentialsId: 'git-token', url: 'https://github.com/alexbuicescu/http-hello-world2')
              sh 'docker-compose build'
              sh 'pwd'
              sh 'ls'
            }

          }
        }
      }
    }
    stage('Starting') {
      parallel {
        stage('Starting Server 1') {
          node('server1') {
            dir(path: 'Server1') {
              sh 'docker-compose up -d'
            }

          }
        }
        stage('Starting Server 2') {
          node('server2') {
            dir(path: 'Server2') {
              sh 'docker-compose up -d'
            }

          }
        }
      }
    }
    stage('Health Checking') {
      parallel {
        stage('Health Checking Server 1') {
          node('server1') {
            dir(path: 'Server1') {
              timeout(time: 30, unit: 'SECONDS') {
                sh './health.sh --url=http://localhost:8000'
              }

            }

          }
        }
        stage('Health Checking Server 2') {
          node('server2') {
            dir(path: 'Server2') {
              timeout(time: 30, unit: 'SECONDS') {
                sh './health.sh --url=http://localhost:8000'
              }

            }

          }
        }
      }
    }
    stage('Testing') {
      node('server1') {
        dir(path: 'Server1') {
          sh 'echo "testing..."'
        }

      }
    }
  }
  post {
    always {
      node('server1') {
        dir(path: 'Server1') {
          sh 'docker-compose down'
        }

      }

      node('server2') {
        dir(path: 'Server2') {
          sh 'docker-compose down'
        }

      }


    }

  }
}