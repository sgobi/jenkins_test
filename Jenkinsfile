opipeline {
    agent any

    environment {
                        echo "enviroment branch-----------------------------------------------------------------------------------------------------------------------------------"

        // Set the Vault address and token to interact with Vault
        VAULT_ADDR = 'http://192.168.1.126:8200'
        VAULT_TOKEN = credentials('vault-token-id') // Jenkins Credential ID for Vault token
        GIT_CREDENTIALS = credentials('git-credentials-id') // Jenkins Credential ID for Git credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                                echo "check out branch-----------------------------------------------------------------------------------------------------------------------------------"

                // Checkout the code from the Git repository using the credentials stored in Jenkins
                git branch: 'hashi',
                    url: 'https://github.com/your-repo.git',
                    credentialsId: 'git-credentials-id'
                echo "hashi branch-----------------------------------------------------------------------------------------------------------------------------------"
            }
        }

        stage('Retrieve Secrets from Vault') {
            steps {
                script {
                                    echo "retrivr start branch-----------------------------------------------------------------------------------------------------------------------------------"

                    // Fetch secret data from HashiCorp Vault
                    // Assuming there's a secret stored at secret/myapp/config in Vault
                    def secretUsername = sh(script: 'vault kv get -field=username secret/myapp/config', returnStdout: true).trim()
                    def secretPassword = sh(script: 'vault kv get -field=password secret/myapp/config', returnStdout: true).trim()
                    
                    // Print the retrieved secrets (avoid printing sensitive data in real-world scenarios)
                    echo "Retrieved secret username: ${secretUsername}"
                    echo "Retrieved secret password: ${secretPassword}"
                    
                    // Set retrieved secrets to environment variables for later use
                    env.SECRET_USERNAME = secretUsername
                    env.SECRET_PASSWORD = secretPassword
                                    echo "retrive end  branch-----------------------------------------------------------------------------------------------------------------------------------"

                }
            }
        }

        stage('Build') {
            steps {
                                echo "build start branch-----------------------------------------------------------------------------------------------------------------------------------"

                // Simulate a build step that uses the secrets retrieved from Vault
                echo "Using Vault secrets for the build..."
                echo "Username: ${env.SECRET_USERNAME}"
                // Do not print sensitive values like passwords
                                echo "build end branch-----------------------------------------------------------------------------------------------------------------------------------"

            }
        }

        stage('Deploy') {
            steps {
                // Simulate a deployment step where secrets might be needed
                echo "Deploying application using the Vault-protected secrets..."
            }
        }
    }

    post {
        always {
            // Clean up or notify at the end of the pipeline
            echo "Pipeline completed."
        }
    }
}
