pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1' // Replace with your desired region
        AWS_ACCESS_KEY_ID = credentials('accesskey')
        AWS_SECRET_ACCESS_KEY = credentials('secretkey')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your version control
                git branch: 'main', url: 'https://github.com/Johithkrishna0110/Terraform/tree/43c0317c83a2479efa8408cc8e4c3fc861d49ed8/module1'
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Create a Terraform plan
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Action') {
            steps {
                script {
                    echo "Terraform action is -> ${action}"
                    sh ("terraform ${action} -auto-approve")
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace or perform any necessary cleanup
            cleanWs()
        }
        success {
            echo 'Terraform applied successfully!'
        }
        failure {
            echo 'Terraform apply failed.'
        }
    }
}