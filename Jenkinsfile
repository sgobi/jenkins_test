pipeline {
    agent any

    environment {
        GIT_URL = "https://github.com/sgobi/jenkins_test.git"
        FILE_TO_PUSH = "myfile.txt"  // Replace with the path of the file you want to push
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch
                git url: "${GIT_URL}", branch: 'main'
            }
        }

        stage('Build') {
            when {
                not { changeRequest() } // Only build on push events
            }
            steps {
                echo "Building the application on push..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            when {
                allOf {
                    branch 'main'   // Only deploy on the main branch
                    not { changeRequest() } // Exclude pull requests
                }
            }
            steps {
                script {
                    echo "Deploying the application..."

                    // List files to check if the target file exists in the workspace
                    sh 'ls -al'

                    // Configure git
                    sh 'git config --global user.email "g2k2@live.com"'
                    sh 'git config --global user.name "sgobi"'

                    // Pull the latest changes from the main branch
                    sh "git pull origin main"

                    // Add and commit the specific file
                    sh "git add ."  // Ensure the file exists
                    sh 'git status'  // Verify the file was added

                    // Commit the changes, but don't fail if there's nothing new to commit
                    sh "git commit -m 'Automated deployment: added poo' || true"

                    // Use withCredentials to safely pass GitHub credentials
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        // Push the changes and show verbose output for debugging
                        sh '''
                            git push -v https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/sgobi/jenkins_test.git main
                        '''
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
            }
        }
    }

    post {
        always {
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
