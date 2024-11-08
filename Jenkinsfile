pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v $WORKSPACE/node_modules:/app/node_modules' // Volume for caching node_modules
                    reuseNode true
                }
            }
            steps{
                sh''' 
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v $WORKSPACE/node_modules:/app/node_modules' // Volume for caching node_modules
                    reuseNode true
                }
            }

            steps{
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
                    args '-v $WORKSPACE/node_modules:/app/node_modules' // Volume for caching node_modules
                    reuseNode true
                }
            }
            steps{
                sh''' 
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
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