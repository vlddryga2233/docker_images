@Library('jenkins-shared-lib') _

pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: docker
                image: docker
                command:
                - cat
                tty: true
            '''
        }
    }

    parameters { 
        choice(name: 'IMAGE', choices: ['iac-builder', 'java-21-builder'], description: 'Select image to build') 
    }

    stages {
        stage('Validate docker version'){
            steps {
                container('docker'){
                    sh 'docker --version'
                }
            }
        }
        stage('Build Docker image'){
            steps {
                container('docker'){
                    sh 'docker build -t vlad1020/${params.IMAGE}:latest ./${params.IMAGE}/'
                }
            }
        }
        stage('Push to Docker Hub'){
            steps {
                container('docker'){
                    sh 'docker push vlad1020/${params.IMAGE}:latest'
                }
            }
        }
    }
}

