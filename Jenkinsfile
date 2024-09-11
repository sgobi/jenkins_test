pipeline {
    agent any

    environment {
        GIT_URL = "https://github.com/sgobi/jenkins_test.git"
       // FILE_TO_PUSH = "myfile.txt"  // File to be updated and pushed
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch
                git url: "${GIT_URL}", branch: 'main'
            }
        }

        stage('Modify File') {
            steps {
                // Make some changes to the file (example)
                echo "Updating file contents..."
               // sh "echo 'New content added at $(date)' >> ${FILE_TO_PUSH}"
            }
        }

        stage('Deploy') {
            when {
                branch 'main'   // Only deploy on the main branch
            }
            steps {
                script {
                    echo "Deploying the application..."

                    // List files to check if the target file exists in the workspace
                    sh 'ls -al'
                    //sh "cat ${FILE_TO_PUSH}"  // Check the contents of the modified file

                    // Configure Git
                    sh 'git config --global user.email "g2k2@live.com"'
                    sh 'git config --global user.name "sgobi"'

                    // Pull the latest changes from the main branch
                    sh "git pull origin main"

                    // Add and commit the specific file
                    sh "git add ."  // Ensure the file is staged
                    sh 'git status'  // Verify the file was added

                    // Commit the changes, but don't fail if there's nothing new to commit
                    sh "git commit -m 'Automated deployment: updated ' || true"

                    // Use withCredentials to safely pass GitHub credentials
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        // Push the changes
                        sh '''
                            git push -v https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/sgobi/jenkins_test.git main
                        '''
                    }
                }
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
