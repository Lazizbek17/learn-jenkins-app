pipeline {
    agent any
    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -ls
                     node --version
                     npm --version
                     npm ci 
                     npm run build
                     ls -ls
                '''
            }
        }
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
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
    }
    
    post {
         always {
            junit 'test-examples/junit.xml'
         }
    }

}
