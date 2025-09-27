pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-webapp"
        CONTAINER_NAME = "demo-web-container"
        GIT_URL = "https://github.com/bharath1911kumar/Project-2.git"  // <-- replace with your repo URL
        GIT_BRANCH = "master"  // change to "master" if your repo uses master
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Jenkins Git plugin will clone the repo fresh each build
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
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

    post {
        failure {
            echo "Build failed! Please check logs."
        }
        success {
            echo "Deployment successful! Running container: %CONTAINER_NAME%"
        }
    }
}
