pipeline {
    agent any

    environment {
        GIT_URL = "https://github.com/sgobi/jenkins_test.git"
        FILES_TO_MERGE = "file1.txt file2.txt myfile.txt"  // Specify the files to merge
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch
                git url: "${GIT_URL}", branch: 'main'
            }
        }

        stage('Modify Files') {
            steps {
                echo "Updating the files..."
                // Modify the specified files (example modifications)
                sh "echo 'New content added at $(date)' >> file1.txt"
                sh "echo 'New content added at $(date)' >> file2.txt"
                sh "echo 'New content added at $(date)' >> myfile.txt"
            }
        }

        stage('Deploy') {
            when {
                branch 'main'  // Only deploy on the main branch
            }
            steps {
                script {
                    echo "Deploying the application..."

                    // List files to verify that the target files exist in the workspace
                    sh 'ls -al'

                    // Configure Git
                    sh 'git config --global user.email "g2k2@live.com"'
                    sh 'git config --global user.name "sgobi"'

                    // Pull the latest changes from the main branch
                    sh "git pull origin main"

                    // Add the modified files
                    sh "git add ${FILES_TO_MERGE}"

                    // Commit the changes; this will not fail the pipeline if there are no changes
                    sh "git commit -m 'Automated deployment: merged changes' || true"

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
