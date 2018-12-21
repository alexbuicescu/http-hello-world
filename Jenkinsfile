pipeline {
  agent none
  stages {
    stage('Pulling') {
      parallel {
        stage('Pull Server 1') {
          agent {
            label 'server1'
          }
          steps {
            dir(path: 'Server1') {
              sh 'ls'
              checkout scm
              sh 'pwd'
              sh 'ls'
              sh 'cat test'
            }

          }
        }
        stage('Pull Server 2') {
          agent {
            label 'server2'
          }
          steps {
            dir(path: 'Server2') {
              git(branch: 'master', credentialsId: 'git-token', url: 'https://github.com/alexbuicescu/http-hello-world2')
              sh 'pwd'
              sh 'ls'
            }

          }
        }
      }
    }
    stage('Building') {
      parallel {
        stage('Build Server 1') {
          agent {
            label 'server1'
          }
          steps {
            dir(path: 'Server1') {
              sh 'pwd'
              sh 'ls'
              sh 'cat test'
            }

          }
        }
        stage('Build Server 2') {
          agent {
            label 'server2'
          }
          steps {
            dir(path: 'Server2') {
              sh 'pwd'
              sh 'ls'
            }

          }
        }
      }
    }
  }
}