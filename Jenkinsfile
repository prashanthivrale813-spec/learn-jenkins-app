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
                    npm ci
                    npm run build
                    ls -la
                '''    
            }
        }

        // 2. Test Stage runs only after Build finishes successfully
        stage('Test') {
            agent { 
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent { 
                docker {                  
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install netlify-cli -g
                    netlify --version
                '''    
            }
        }
}