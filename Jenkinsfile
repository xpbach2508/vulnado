pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Add your build steps here
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                // Add your test steps here
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                // Add your deploy steps here
                echo 'Deploying...'
            }
        }
    }
    post {
        always {
            // Steps to always run after the pipeline, like cleanup or notifications
            echo 'Pipeline completed.'
        }
    }
}
