pipeline {
    agent any

    // environment{
    //     NETLIFY_SITE_ID  = '197fc7f6-0a5d-4a42-a44a-d4ebc3d34c89'
    //     NETLIFY_AUTH_TOKEN = credentials('jenkins-token')
    // }

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
            

   
        stage('Deploy'){
            agent{
                docker{
                    image 'amazon/aws-cli'
                    reuseNode true
                    args '--entrypoint=""'
                }
            }

             steps {
                withCredentials([usernamePassword(credentialsId: 'demo-credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                            aws --version
                            aws s3 ls

                            # echo "hello S3!" > index.html
                            # aws s3 cp index.html s3://demo250403/index.html

                        '''
                }
            }
        }
    


        // }
        
        // stage('Deploy') {
        //     agent{
        //         docker{
        //             image 'node:22.14.0-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             npm install netlify-cli
        //             node_modules/.bin/netlify --version
        //             echo "Deploying to production. Site ID: $NETLIFY_SITE_ID."
        //             node_modules/.bin/netlify status
        //             node_modules/.bin/netlify deploy --prod --dir=build
        //         '''  
        //     }
        // }
    }  
}