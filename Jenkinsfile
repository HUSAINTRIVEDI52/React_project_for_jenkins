pipeline {

    
    agent any
     {
        options{
            skipDefaultCheckout(true)
        }
        docker {
            image 'node:20-alpine'
            args '-u root:root'
        }
    }

    stages {

        stage('Checkout using SCM'){
            steps{
                checkout scm
            }

        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing NPM dependencies...'
                sh 'npm ci'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm test -- run'
            }
        }
        */

        stage('Build Application') {
            steps {
                echo 'Building the React application...'
                sh 'npm run build'
            }
        }

       
    }

    post {
        always {
            echo 'Cleaning up the workspace...'
            cleanWs()
        }
    }
}

