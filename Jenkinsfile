pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the Git repository
                git url: 'https://github.com/xpbach2508/WebGoat.git', branch: 'master'
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
        // stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }
        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }
        stage('Sonarqube scanner') {
            steps {
               script {
                    withSonarQubeEnv(installationName='sonar') {
                        sh 'mvn sonar:sonar'
                    }
               }
            }
            post {
                always {
                    script {
                    def getURL = readProperties file: './target/sonar/report-task.txt'
                    def taskId = getURL['ceTaskId']
                    withCredentials([string(credentialsId: 'sonarqube_admin_secret', variable: 'SONARQUBE_TOKEN')]) {
                        withSonarQubeEnv(installationName='sonar') {
                            sh "curl -u ${SONARQUBE_TOKEN}: '${SONAR_HOST_URL}/api/ce/task?id=${taskId}' > sonar-report.json"
                        }
                    }
                    // def qg = waitForQualityGate()
                    // if (qg.status != 'OK') {
                    //     echo "failure: ${qg.status}"
                    //     def getURL = readProperties file: './target/sonar/report-task.txt'
                    //     echo ${getURL['ceTaskId']}
                    // }
                    }
                }
            }
        }
        stage('Spotbugs') {
            steps {
                sh 'mvn spotbugs:spotbugs'
            }
            post {
                failure {
                    sh 'mvn spotbugs:gui'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                recordIssues enabledForFailure: true, tool: spotBugs()
                recordIssues enabledForFailure: true, tool: sonarQube()
            }
        }
    }
}