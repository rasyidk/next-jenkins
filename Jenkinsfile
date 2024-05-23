pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html/study/next-jenkins'
        PM2_APP_NAME = 'next-jenkins-app' // Name of your PM2 app
    }

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/rasyidk/next-jenkins.git'
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build the app') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Copy the built application to the deployment directory
                    sh "rsync -av --delete ${WORKSPACE}/ ${DEPLOY_DIR}/"

                    // Start or reload the application using PM2
                    sh """
                        cd ${DEPLOY_DIR}
                        if pm2 list | grep -q ${PM2_APP_NAME}; then
                            pm2 reload ${PM2_APP_NAME}
                        else
                            pm2 start npm --name "${PM2_APP_NAME}" -- start
                        fi
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
