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

        stage('Cleanup Old Containers and Images') {
            steps {
                script {
                    // Stop and remove all existing containers
                    sh '''
                    docker ps -q | xargs -r docker stop
                    docker ps -a -q | xargs -r docker rm
                    '''
                    
                    // Remove all unused Docker images
                    sh 'docker images -q | xargs -r docker rmi -f'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build the new Docker image
                    sh 'docker build -t react-app:latest .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Run the new Docker container
                    sh 'docker run -d -p 3000:3000 react-app:latest'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up dangling (unused) containers
                sh 'docker container prune -f'
                // Optionally, clean up dangling images
                sh 'docker image prune -f'
            }
        }
    }
}
