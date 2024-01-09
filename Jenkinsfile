pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'musrafulhaque/sample-react-application'
        DOCKER_REGISTRY = 'https://registry-1.docker.io/v2/'
        DOCKER_CREDENTIALS = 'musrafulhaque'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Tag') {
            steps {
                script {
                    def gitCommitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    sh "docker tag $DOCKER_IMAGE $DOCKER_IMAGE:$gitCommitHash"
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DockerHub', variable: 'Dockerhub')]) {
                        sh "docker login -u $DOCKER_CREDENTIALS -p $DockerHub"
                    }
                    def gitCommitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()

                    sh "docker push $DOCKER_IMAGE:$gitCommitHash"
                }
            }
        }
    }
}

