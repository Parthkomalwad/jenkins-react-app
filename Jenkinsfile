pipeline {
    agent any
    tools { nodejs "NODEJS" }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Parthkomalwad/jenkins-react-app.git'
            }
        }
        stage('Verify Docker Access') {
            steps {
                sh 'docker --version'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t react-app:latest .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 3000:3000 react-app:latest'
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up old containers
                sh 'docker container prune -f'
            }
        }
    }
}
