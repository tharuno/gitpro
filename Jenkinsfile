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
                    echo "Waiting for application to start..."

                    i=1

                    while [ "$i" -le 12 ]
                    do
                        if curl --fail --silent --show-error http://localhost:8080/api/employees
                        then
                            echo ""
                            echo "Application is healthy!"
                            exit 0
                        fi

                        echo "Application not ready yet. Attempt $i/12"

                        i=$((i + 1))
                        sleep 5
                    done

                    echo "Application failed to become healthy."
                    exit 1
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
