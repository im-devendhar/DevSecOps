pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/example/repo.git'
            }
        }

        stage('SAST - SonarQube') {
            steps {
                echo "Running SAST scan"
                sh 'sonar-scanner'
            }
        }

        stage('SCA - Dependency Check') {
            steps {
                echo "Running SCA scan"
                sh 'dependency-check.sh --project my-app --scan .'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }

        stage('Container Scan - Trivy') {
            steps {
                sh 'trivy image my-app:latest'
            }
        }

        stage('Push to Registry') {
            steps {
                sh 'docker push my-app:latest'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application"
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                echo "Running DAST scan"
                sh 'zap-cli quick-scan http://my-app-url'
            }
        }
    }
}
