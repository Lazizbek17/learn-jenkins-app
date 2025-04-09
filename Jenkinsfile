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
                    echo "Build bosqichi"
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -ls build
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
                    echo "Test bosqichi"
                    test -f build/index.html
                    npm test -- --testResultsProcessor=jest-junit
                '''
            }

            post {
                always {
                    junit 'test-results/junit.xml'
                }
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
}
