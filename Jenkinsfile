pipeline {
    agent any

    tools {
        maven 'Maven'  // Configure Maven in Jenkins under Global Tool Configuration
        jdk 'JDK 17'   // Configure JDK 17 in Jenkins
    }

    stages {
        stage('Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/frank241/pet-clinic.git', branch: 'main'
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
                    docker.build('spring-petclinic')
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
