pipeline {
    agent any

    stages {
        /*
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

                '''
            }
        }
        */
        stage('parallel stages'){
            parallel{
                stage('Test'){
             agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo "Testing the build.."
                sh '''
                   #test -f build/index.html
                   npm test
                ''' 
            }
        }
        stage('E2E'){
             agent{
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                echo "Testing the build.."
                sh '''
                   npm install serve
                   node_modules/.bin/serve -s build &
                   sleep 10
                   npx playwright test --report=html
                '''   
            }
        }
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}