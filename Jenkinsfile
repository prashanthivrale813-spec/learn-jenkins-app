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
                    # Ensure build folder exists before testing
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Install & Verify Netlify') {
            agent { 
                docker {                   
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Install globally so 'netlify' command is available
                    npm install -g netlify-cli
                    
                    # Verify installation
                    node_module/.bin/netlify --version
                '''    
            }
        }
    }
}