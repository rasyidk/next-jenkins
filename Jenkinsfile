pipeline {
    agent any

    tools {
        // Install Node.js named "NodeJS 21"
        nodejs 'nodejs'
    }

    environment {
        DEPLOY_DIR = '/var/www/html/study/next-jenkins'
        PM2_APP_NAME = 'next-jenkins-app' // Name of your PM2 app
    }

    stages {
        stage('Check node') {
            steps {
                sh 'node -v'
                sh '/var/lib/jenkins/.yarn/bin/pm2 status'
            }
        }

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
                        
                        if /var/lib/jenkins/.yarn/bin/pm2 list; then
                            /var/lib/jenkins/.yarn/bin/pm2 reload name next-jenkins-app
                        else
                            /var/lib/jenkins/.yarn/bin/pm2 start npm --name name next-jenkins-app -- start
                        fi
                    """
                    // sh "/var/lib/jenkins/.yarn/bin/pm2 start npm --name next-jenkins-app -- start"
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
