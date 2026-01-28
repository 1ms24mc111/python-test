pipeline {
    agent any
    environment {
        // This must match the ID you gave your credentials in Jenkins Settings
        DOCKERHUB_CRED = credentials('dockerhubID') 
        IMAGE_NAME = 'vivek5041/python-test'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Using the HTTPS URL you provided
                git url: 'https://github.com/1ms24mc111/python-test.git', branch: 'main' 
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Building with the 'latest' tag
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                script {
                    // This logs into Docker Hub using your Jenkins credentials
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhubID') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Successfully pushed ${IMAGE_NAME} to Docker Hub!"
        }
        failure {
            echo "Pipeline failed. Check the Jenkins Console Output."
        }
        always {
            deleteDir() // Keeps your Jenkins server clean
        }
    }
}
