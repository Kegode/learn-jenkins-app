pipeline{
    agent any
    
    environment{
        NETLIFY_SITE_ID = '5298ebd3-930f-4b09-82ae-13631c23745b'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    
    stages{
        stage('Build'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
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
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
                steps{
                    sh '''
                    echo "Test Stage"
                    test -f build/index.html
                    npm test
                    '''
                }
        }
         stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                     sh '''
                       npm install netlify-cli@20.1.1
                       node_modules/.bin/netlify --version
                       echo "Deploy the app to Site ID: $NETLIFY_SITE_ID"
                       node_modules/.bin/netlify status
                     '''
            }
        }

    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}