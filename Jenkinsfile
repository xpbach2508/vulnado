pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/xpbach2508/vulnado.git', branch: 'master'
            }
        }
        stage ('Prepare') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Sonarqube scanner') {
            steps {
                withSonarQubeEnv(installationName='sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Done'
            }
        }
    }
}