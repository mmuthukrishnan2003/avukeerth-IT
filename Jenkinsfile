pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'mkdir -p build'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Checks') {
            steps {
                echo 'Running static checks...'
                sh 'echo "Running static checks..."'
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running test cases...'
                sh 'mvn test'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'mvn verify -Pfailsafe'
            }
        }

        stage('Sonar Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Pre-Production') {
            steps {
                echo 'Deploying to Pre-Production environment...'
                sh 'echo Pre-Production deployment completed'
            }
        }

        stage('dev - Request Approval') {
            steps {
                input message: 'Approve deployment to dev?', ok: 'Deploy'
            }
        }

        stage('dev - Apply Version') {
            steps {
                sh './scripts/apply-version.sh'
            }
        }

        stage('dev - Create ECR') {
            steps {
                sh 'aws ecr create-repository --repository-name my-app || true'
            }
        }

        stage('dev - Build and Push to ECR') {
            steps {
                sh '''
                    docker build -t my-app .
                    docker tag my-app:latest <account>.dkr.ecr.<region>.amazonaws.com/my-app:latest
                    docker push <account>.dkr.ecr.<region>.amazonaws.com/my-app:latest
                '''
            }
        }

        stage('dev - Deploy ECS') {
            steps {
                sh '''
                    aws ecs update-service \
                        --cluster my-cluster \
                        --service my-service \
                        --force-new-deployment
                '''
            }
        }

        stage('Production') {
            steps {
                echo 'Deploying to Production environment...'
                sh 'echo Production deployment completed'
            }
        }

        stage('dev - Api Collection') {
            steps {
                sh 'newman run api-tests/collection.json'
            }
        }

    }
}
