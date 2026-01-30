pipeline {
    agent any

    stages {
        // 1. Build Stage runs first
        stage('Build') {
            agent { 
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node --version
                    npm ci
                    npm run build
                '''    
            }
        }

        // 2. Test Stage runs only after Build finishes successfully
        stage('Test') {
            steps {
                echo 'Test stage'
            }
        }
    }
}