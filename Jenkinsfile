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
                        dir('Server2') {
                            git branch: 'master',
                                credentialsId: 'git-token',
                                url: 'https://github.com/alexbuicescu/http-hello-world2'
                            sh 'pwd'
                            sh 'ls'
                        }
                    }
                }
            }
        }
    }

    stages {
        stage ('Building') {
            parallel {
                stage('Build Server 1') {
                    agent {
                        label 'server1'
                    }
                    steps {
                        dir('Server1') {
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
                        dir('Server2') {
                            sh 'pwd'
                            sh 'ls'
                        }
                    }
                }
            }
        }
    }
}