pipeline {
    agent any
    environment {
        // This MUST match the ID in Jenkins -> Manage Jenkins -> Credentials
        DOCKERHUB_CRED = credentials('dockerhubID') 
        IMAGE_NAME = 'vivek5041/python-test'
	PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/1ms24mc111/python-test.git', branch: 'main' 
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Using sh is safer on Mac than the docker.build script block
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                // Use the environment variables created by 'credentials' to login and push
                sh "echo ${DOCKERHUB_CRED_PSW} | docker login -u ${DOCKERHUB_CRED_USR} --password-stdin"
                sh "docker push ${IMAGE_NAME}:latest"
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
            sh "docker logout" // Security best practice
            deleteDir() 
        }
    }
}
