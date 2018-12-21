pipeline {
    agent none
    stages {
        stage ('Pulling') {
            parallel {
                stage('Pull Server 1') {
                    agent {
                        label 'server1'
                    }
                    steps {
                        dir('Server1') {
                            checkout scm
                        }
                    }
                }

                stage('Pull Server 2') {
                    agent {
                        label 'server2'
                    }
                    steps {
                        dir('Server2') {
                            git branch: 'master',
                                credentialsId: 'git-token',
                                url: 'https://github.com/alexbuicescu/http-hello-world2'
                        }
                    }
                }
            }
        }
    }
}