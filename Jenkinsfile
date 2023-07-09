pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_DEFAULT_REGION    = 'ap-south-1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/CodeSagarOfficial/jenkins-scripts.git'
            }
        }
        stage('Terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform action') {
            steps {
                script {
                    if (params.action == 'apply') {
                        sh 'terraform plan -out tfplan'
                        sh 'terraform show -no-color tfplan > tfplan.txt'
                        sh 'terraform ${action} --auto-approve tfplan'
                    }
                    else if (params.action == 'destroy') {
                        sh 'terraform ${action} --auto-approve'
                    }
                }
            }
        }
    }
}