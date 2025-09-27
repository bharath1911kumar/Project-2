pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-webapp"
        CONTAINER_NAME = "demo-web-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                bat """
                cd %WORKSPACE%
                git pull origin main
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .
                """
            }
        }

        stage('Stop Old Container') {
            steps {
                bat """
                docker rm -f %CONTAINER_NAME% 2>nul || echo No existing container to remove
                """
            }
        }

        stage('Run New Container') {
            steps {
                bat """
                docker run -dit --name %CONTAINER_NAME% -p 8082:80 %IMAGE_NAME%:%BUILD_NUMBER%
                """
            }
        }

        stage('Cleanup Images') {
            steps {
                bat """
                docker image prune -f
                """
            }
        }
    }
}
