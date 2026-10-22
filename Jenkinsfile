pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Docker') {
            steps {
                sh 'docker --version'
                sh 'docker compose version'
            }
        }

        stage('Build Images') {
            steps {
                sh 'docker compose -p book-recommender build'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker compose -p book-recommender up -d'
            }
        }

        stage('Verify Containers') {
            steps {
                sh 'docker compose -p book-recommender ps'
            }
        }

        stage('Application Test') {
            steps {
                sh 'sleep 5'
                sh 'curl --fail --silent --show-error http://localhost/api/books'
            }
        }
    }

    post {
        success {
            echo 'Book-Recommender CI/CD completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Review Console Output.'
        }
    }
}
