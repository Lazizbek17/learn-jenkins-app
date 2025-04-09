pipeline {
    agent any

    stages {
        // Build stage
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

        // Unit Test stage
      stage('Unit Test') {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }
    steps {
        sh '''
            echo "Unit Test bosqichi"
            test -f build/index.html
            npm test -- --testResultsProcessor=jest-junit
        '''
    }
    post {
        always {
            junit '**/test-results/junit.xml'
        }
    }
}


        // E2E Test stage
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "E2E Test bosqichi"
                    npm install serve
                    serve -s build &
                    sleep 10
                    npx playwright test --reporter=html
                '''
            }
            post {
                always {
                    publishHTML(
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: false,
                        reportDir: 'playwright-report',
                        reportFiles: 'index.html',
                        reportName: 'Playwright Test Report'
                    )
                }
            }
        }

        // Deploy stage
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Deploy bosqichi"
                    npm install netlify-cli -g
                    netlify --version
                    netlify deploy --dir=build --prod
                '''
            }
        }
    }
}
