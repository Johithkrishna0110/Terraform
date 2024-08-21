pipeline {
    agent any

    environment {
        TF_WORKSPACE = 'Pre-Prod'
        TF_ORG = 'johithsorg'
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
                    // Write the token to the Terraform credentials file
                    sh '''
                    mkdir -p ~/.terraform.d
                    echo '{ "credentials": { "app.terraform.io": { "token": "'$TF_API_TOKEN'" } } }' > ~/.terraform.d/credentials.tfrc.json
                    '''
                    
                    // Initialize Terraform
                    sh 'terraform init'
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