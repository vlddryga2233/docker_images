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
                image: docker:20.10-dind
                securityContext:
                    privileged: true
                env:
                - name: DOCKER_TLS_CERTDIR
                  value: ""
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
        stage('Login to Docker Hub'){
            steps {
                container('docker'){
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                            usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }
                }
            }
        }
        stage('Build Docker image'){
            steps {
                container('docker'){
                    sh "docker build -t vlad1020/${params.IMAGE}:latest ./${params.IMAGE}/"
                }
            }
        }
        stage('Push to Docker Hub'){
            when {
                branch 'main'
            }
            steps {
                container('docker'){
                    sh "docker push vlad1020/${params.IMAGE}:latest"
                }
            }
        }
    }
}

