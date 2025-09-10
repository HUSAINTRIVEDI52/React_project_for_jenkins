pipeline {
    agent any
    stages {
       stage('Clean Workspace') {
          steps {
            deleteDir()
         }
      }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true // Reuse the node for the next stages
                }
            }

            steps {
                sh '''
                    ls -l
                    node --version
                    npm --version
                     npm install --cache .npm --prefer-offline
                    npm run build
                    ls -l
                '''
            }
        }
    }
}