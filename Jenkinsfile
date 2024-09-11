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
                // For example, run unit tests or integration tests
                //sh './run_tests.sh'
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
                echo "pulling "
                    // Ensure the workspace is up to date
                 //    sh 'git pull origin main'  // Pull the latest changes
                  sh  ' git branch -u origin/main'

               //  sh 'git checkout --orphan main'
                    // // Add, commit, and push the file(s)
                     sh 'git add -A '             // Add all files
                     sh 'git commit -am "awt"'  // Commit changes
                    
                    // // Push changes back to the repository
                     sh 'git push origin origin/main' 
                }
            }
        }

        stage('Pull Request Validation') {
            when {
                changeRequest()  // Run this stage for pull requests only
            }
            steps {
                echo "Validating pull request..."
                // Add your pull request validation steps, such as running additional tests
                //sh './run_tests.sh'
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
