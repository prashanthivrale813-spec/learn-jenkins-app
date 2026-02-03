pipeline {
    agent any

    stages {
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
                // Using npx avoids the 'Permission Denied' error
                sh 'npx netlify-cli --version' 
            }
        }
    } // End of stages
} // End of pipeline