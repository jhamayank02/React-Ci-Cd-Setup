pipeline {
    agent any
    environment {
        MY_VAR = 'my value'
    }
    options {
        skipDefaultCheckout(true) // Skip the default checkout
    }
    stages {
        stage ('clean up code'){
            steps{
                cleanWs()
            }
        }
        stage ('checkout using SCM'){
            steps {
                checkout scm // Checkout the code
            }
        }
        stage("Take approval"){
            steps {
                input 'Should we deploy?'
            }
        }
        stage ('Build') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true // Reuse the node for the next stages
                }
            }

            steps {
                    sh '''
                        ls -l
                        node --version
                        npm --version
                        npm install
                        npm run build
                        ls -l
                    '''
            }
        }
        stage ('Deploy'){
            agent {
                docker {
                    er {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true // Reuse the node for the next stages
                }
            }

            steps {
                sh '''
                    npm install -g vercel
                    echo $MY_VAR
                '''
            }
        }
    }
    }
}