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

                stage ('ECE') {
            agent { 
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test

                '''
            }
        }
    }
    
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}