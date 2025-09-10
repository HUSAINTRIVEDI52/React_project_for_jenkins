pipeline {
    // 1. Define the build agent
    agent {
        docker {
            // Use a lightweight and modern version of Node. 'alpine' images are smaller.
            image 'node:20-alpine'
            // CRITICAL FIX: Run the container as the root user to avoid permission errors
            // when Jenkins mounts the workspace volume.
            args '-u root:root'
        }
    }

    stages {
        // 2. Install dependencies cleanly and reproducibly
        stage('Install Dependencies') {
            steps {
                echo 'Installing NPM dependencies...'
                // Use 'npm ci' instead of 'npm install'. It's faster and safer for CI/CD
                // because it installs exact versions from your package-lock.json file.
                sh 'npm ci'
            }
        }

        // 3. Run automated tests to ensure code quality
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                // The '-- --watchAll=false' argument is crucial for Create React App.
                // It tells the test runner to execute all tests once and then exit,
                // instead of entering the interactive "watch" mode.
                sh 'npm test -- --watchAll=false'
            }
        }

        // 4. Build the production-ready application
        stage('Build Application') {
            steps {
                echo 'Building the React application...'
                // This command bundles your app into static files for production.
                // The output will be in the 'build' directory.
                sh 'npm run build'
            }
        }

        // 5. Archive the build output for deployment
        stage('Archive Artifacts') {
            steps {
                echo 'Archiving the build directory...'
                // This saves the entire 'build' folder as an artifact of this Jenkins job.
                // You can then use this artifact in a separate deployment job.
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    // 6. Always clean up the workspace at the end
    post {
        always {
            echo 'Cleaning up the workspace...'
            // This ensures that the next build starts in a completely clean environment.
            cleanWs()
        }
    }
}
