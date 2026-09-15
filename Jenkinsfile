pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    echo "Checking Docker..."
                    docker --version

                    echo "Checking Docker Compose..."
                    docker compose version

                    echo "Checking project output..."
                    ls -la
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    docker compose build
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker compose up -d
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker compose ps

                    echo "Testing application..."
                    curl http://localhost/ || exit 1

                    echo "Testing API..."
                    curl http://localhost/api/employees || exit 1
                '''
            }
        }
    }

    post {

        success {
            echo 'Wow Your Deployment successful!'
        }

        failure {
            echo 'Sorry Deployment failed!'
            sh 'docker compose logs --tail 100 || true'
        }
    }
}
