pipeline{
    agent any

    environment{
        NETLIFY_SITE_ID = '253a3bfb-a8ae-4e86-a950-de99624a3635'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages{
        stage('Deploy staging'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                npm install netlify-cli@20.1.1 node-jq
                node_modules/.bin/netlify --version
                echo "Deploying to Staging. Site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --json > deploy-output.json
                node_modules/.bin/node-jq -r '.deploy_url' deploy-output.json
                '''
            }
        }

        stage ('Approval'){
            steps{
<<<<<<< HEAD
                timeout(time: 1, unit: 'MINUTES') {
                    input message: 'Do you wish to deploy to production?', ok: 'Yes, I am sure'
                    }
                
=======
                timeout(time: 15, unit: 'MINUTES')
                input message: 'Do you wish to deploy to production?', ok: 'Yes, I am sure'
>>>>>>> parent of 349ff14 (updated timout)
                
            }
        }

        stage('Deploy Prod'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                npm install netlify-cli@20.1.1
                node_modules/.bin/netlify --version
                echo "Deploying to Production. Site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}