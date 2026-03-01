pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        
        stage('Test') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Test stage"
                    [ -f build/index.js ] && echo "Exists"
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent{
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.2-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g serve
                    serve -s build
                    # start the test
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
