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

        stage('Deploy') {
            steps {
                sh '''
                    sudo -n /usr/local/bin/deploy-employee-management
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    sleep 5
                    curl --fail --silent --show-error \
                         http://localhost:8080/api/employees
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
