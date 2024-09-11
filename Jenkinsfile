pipeline {
    agent any

    environment {
        GIT_URL = "https://github.com/sgobi/jenkins_test.git"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code
                checkout scm
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
                
                // Add your testing steps here, for example:
                // Run unit tests or integration tests
                //sh 'my-pipeline_main tests/'
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
            sh 'git config --global user.name "sgobi "'                   // Set Git user name
            
            // Ensure the workspace is up to date
            sh 'git pull origin main'
            
            // Add, commit, and push the file
            sh 'git add .'     // Replace <file> with the actual file path
            sh 'git commit -m "Automated deployment: added my1"'  // Replace <file> or customize the message
            sh 'git push origin main'  // Push changes back to the repository
        }
    }
}






        

        stage('Pull Request Validation') {
            when {
                changeRequest()  // Run this stage for pull requests only
            }
            steps {
                echo "Validating pull request..."
                // Pull request validation steps, such as running additional tests
           //     sh 'my-pipeline_main tests/'
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
