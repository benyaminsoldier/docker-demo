pipeline {
    agent any
/*
    enviroment{
        NETLIFY_SITE_ID 'd13accd6-ef29-4757-a3f1-06cde5e197b8'
        NETLIFY_AUTH_TOKEN 'nfp_dEpCDWDTdSqmC6X4ngmGZWGhDZsqPAQHc888'
    }*/

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:22.14.0-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm install
                    npm run build
                    ls -la
                '''  
            }
        }
        stage('Test') {
            agent{
                docker{
                    image 'node:22.14.0-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    # grep -R "Daniel Benjumea" . --exclude=Jenkinsfile || (echo "ERROR: Name not found!" && exit 1)
                    npm test
                    # Check if "Daniel Benjumea" is in the source code
                    if grep -riq "Daniel Benjumea" src/; then
                        echo "Daniel Benjumea found in the source code."
                    else
                        echo "ERROR: Daniel Benjumea not found!" >&2
                        exit 1
                    fi
                '''  
            }
        }
        /*
        stage('Deploy') {
            agent{
                docker{
                    image 'node:22.14.0-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID."
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --prod --dir=build
                '''  
            }
        }*/
    }
}