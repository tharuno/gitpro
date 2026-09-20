pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                    cd backend
                    mvn clean compile
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    cd backend
                    mvn test
                '''
            }
        }

        stage('Package') {
            steps {
                sh '''
                    cd backend
                    mvn package -DskipTests
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar',
                                 fingerprint: true
            }
        }
    }
}
