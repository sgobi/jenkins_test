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
                    branch 'main'   // Only deploy on the main branch
                    not { changeRequest() } // Not a pull request
                }
            }
            steps {
                script {
                    echo "Deploying the application..."
                    sh 'git config --global user.email "g2k2@live.com"'   // Set Git user email
                    sh 'git config --global user.name "sgobi"'            // Set Git user name

                    // Ensure the main branch is up to date
                    sh "git pull origin main"


echo "pwd"
                        // Print current directory and list files
            sh 'pwd'
            sh 'ls -la'

                    echo "-------------------------------"

                    

                    // Add and commit the specific file
                    sh "git add ${FILE_TO_PUSH}"  // Add the specific file
                    sh "git commit -m 'Automated commit: Pushed ${FILE_TO_PUSH} to main'"  // Commit the specific file

                    // Use the stored credentials to push changes
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/sgobi/jenkins_test.git main"
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
