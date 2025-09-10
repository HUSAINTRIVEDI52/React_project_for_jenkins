pipeline {
    // 1. Define the execution environment
    agent {
        docker {
            image 'node:18'
            // This is the key fix for the permissions error.
            // It runs the container as the root user.
            args '-u root:root'
        }
    }

    stages {
        // 2. Install dependencies securely and quickly
        stage('Install Dependencies') {
            steps {
                // Use 'npm ci' for CI environments. It's faster and more reliable
                // than 'npm install' as it uses the package-lock.json.
                sh 'npm ci'
            }
        }

        // 3. Run tests to ensure code quality
        stage('Run Tests') {
            steps {
                // This command assumes you have a 'test' script in your package.json
                sh 'npm test'
            }
        }

        // 4. Create the production build
        stage('Build Application') {
            steps {
                // This command creates the optimized static files in the 'build' directory.
                sh 'npm run build'
            }
        }

        // 5. Archive the build output for deployment
        stage('Archive Artifacts') {
            steps {
                // This saves the contents of the 'build' directory as a build artifact
                // that you can download or use in a later deployment stage.
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    // 6. Clean up the workspace after the pipeline finishes
    post {
        always {
            // cleanWs() cleans the workspace to ensure the next build is fresh.
            cleanWs()
        }
    }
}