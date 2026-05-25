pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'mkdir -p build'
            }
        }

        stage('Testing') {
            steps {
                echo 'Running test cases...'
                sh 'echo Testing completed successfully'
            }
        }

        stage('Pre-Production') {
            steps {
                echo 'Deploying to Pre-Production environment...'
                sh 'echo Pre-Production deployment completed'
            }
        }

        stage('Production') {
            steps {
                echo 'Deploying to Production environment...'
                sh 'echo Production deployment completed'
            }
        }
    }
}
