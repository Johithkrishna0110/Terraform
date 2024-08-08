pipeline {
    agent any

    environment {
        TF_WORKSPACE = 'your-workspace'
        TF_ORG = 'your-organization'
        TF_API_TOKEN = credentials('terraform-cloud-api-token')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your version control
                git branch: 'terraform', url: 'https://github.com/Johithkrishna0110/Terraform.git'
            }
        }

        stage('Initialize Terraform') {
            steps {
                script {
                    sh '''
                        echo $TF_API_TOKEN | terraform login
                        terraform init -backend-config="organization=${TF_ORG}" -backend-config="workspaces.name=${TF_WORKSPACE}"
                    '''
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