pipeline {
    agent any

    environment {
        IMAGE_NAME = "task-tracker"
    }

    stages {

        // Pull the latest code from GitHub
        stage('SCM Pull') {
            steps {
                checkout scm
            }
        }

        // Install Node.js packages
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // Run test cases
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        // Build Docker image
        stage('Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        // Start the application using Docker Compose
        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }

        // Check if the application is running
        stage('Curl') {
            steps {
                sh '''
                sleep 10
                curl http://localhost:3000/health
                '''
            }
        }

        // Stop containers and clean Jenkins workspace
        stage('Cleanup') {
            steps {
                sh 'docker compose down'
                sh 'docker image prune -f'
                cleanWs()
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }

        failure {
            echo "Pipeline failed!"
        }

        always {
            echo "Pipeline execution finished."
        }
    }
}
