pipeline {
    agent {
        docker { image 'node:18' 
                args '-u root:root' 

        }
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
    }
}
