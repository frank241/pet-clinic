pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK 17'
        jfrog 'jfrog-cli'   
    }
    environment {
        IMAGE_NAME = "spring-petclinic"
        IMAGE_TAG = "${BUILD_ID}"
        DOCKER_REPO = "jftest2.jfrog.io/jftest2-docker"
    }

    stages {
        stage('Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/frank241/pet-clinic.git'
            }
        }

        stage('Compile Code') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package as Jar') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}", '.')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                dir('docker-oci-examples/docker-example/'){
                   jf 'rt docker-push ${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}'
                }
            }
        }

    }
    
    post {
        always {
            echo 'Pipeline complete.'
        }
    }
}
