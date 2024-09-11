pipeline {
    agent any

    environment {
        GIT_URL = "https://github.com/sgobi/jenkins_test.git"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the specified repository and branch
                git url: "${GIT_URL}", branch: 'main'
            }
        }

        stage('Build') {
            when {
                not { changeRequest() } // Only build on push events
            }
            steps {
                echo "Building the application on push..."
                // Add build steps, e.g., Docker build or PHP build
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Add your testing steps here
            }
        }

        stage('Deploy') {
            when {
                allOf {
                    branch 'main'   // Only deploy on pushes to the main branch
                    not { changeRequest() } // Not a pull request
                }
            }
            steps {
                script {
                    echo "Deploying the application..."
                    sh 'git config --global user.email "g2k2@live.com"'   // Set Git user email
                    sh 'git config --global user.name "sgobi"'            // Set Git user name
                    
                    // Ensure the workspace is up to date
                    sh 'git pull origin main'  // Pull the latest changes
                    
                    // Add, commit, and push the file(s)
                    sh 'git add .'             // Add all files
                    sh 'git commit -a -m "Changes pushed by Jenkins" || true'  // Commit changes

                    // Use the stored credentials to push changes
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/sgobi/jenkins_test.git main'
                    }
                }
            }
        }

        stage('Pull Request Validation') {
            when {
                changeRequest()  // Run this stage for pull requests only
            }
            steps {
                echo "Validating pull request..."
                // Add your pull request validation steps
            }
        }
    }

    post {
        always {
            // Post-build actions, such as cleaning up or sending notifications
            echo "Cleaning up..."
        }
        success {
            echo "Build successful!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
