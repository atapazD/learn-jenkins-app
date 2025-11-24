pipeline{
    agent any
    stages{
        // stage("build"){
        //     agent {
        //         docker{
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps{
        //         sh '''
        //         ls -la
        //         node --version
        //         npm --version
        //         npm ci
        //         npm run build
        //         ls -la
        //         '''
        //     }
        // }
        stage("test"){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                echo "Test Stage..."
                test -f build/index.html
                npm test
                '''
            }
            
        }
        stage("E2E Test"){
            agent {
                docker{
                    image 'mcr.microsoft.com/playwirhgt:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    serve -s build
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