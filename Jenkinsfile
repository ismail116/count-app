
pipeline {
    agent any
    environment {
        PATH = "/var/jenkins_home/bin:$PATH"
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    
    stages {
        stage('Build Dockerfile') {
            steps {
                script {
                    // Build Dockerfile
                    sh 'docker build -t myapp .'
                }
            }
        }
        // stage('Checkout') {
        //     steps {
        //         // Checkout the code from your version control system (e.g., Git)
        //         git branch: 'main', url: 'https://github.com/ismail116/count-app.git'
        //     }
        // }
        stage('Push to ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 949609334280.dkr.ecr.us-east-1.amazonaws.com'
                    }
                    // Tag the image
                    sh 'docker tag myapp:latest 949609334280.dkr.ecr.us-east-1.amazonaws.com/myapp:latest'
                    // Push the image to ECR
                    sh 'docker push 949609334280.dkr.ecr.us-east-1.amazonaws.com/myapp:latest'
                }
            }
        }


        // stage('Deploy to Kubernetes') {
        //     steps {
        //         script {
        //             // Assuming you have kubectl configured on Jenkins
        //             // Deploy the new image to Kubernetes pods
        //             sh 'kubectl set image deployment/myapp-deployment myapp=949609334280.dkr.ecr.us-east-1.amazonaws.com/myapp:latest'
        //         }
        //     }
        // }

    } //stages end

    post {
        success {
            // Send success notifications (e.g., Slack, email) if the pipeline succeeds
            echo 'Pipeline succeeded!'
        }
        failure {
            // Send failure notifications (e.g., Slack, email) if the pipeline fails
            echo 'Pipeline failed!'
        }
    }
} //pipeline end

