pipeline {
    agent any

    stages {
   stage('Checkout') {
        steps {
          // Get some code from a GitHub repository
          git branch: 'main', url: 'https://github.com/QA-Instructor/react-jenkinsfile.git'
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
    }
}