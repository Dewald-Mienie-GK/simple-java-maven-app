pipeline {
    agent none
    
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    customWorkspace "workspace/${JOB_NAME}/backend"
                }
            }
            
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    customWorkspace "workspace/${JOB_NAME}/backend"
                    reuseNode true
                }
            }
            
            steps {
                sh 'mvn test'
            }
            
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    customWorkspace "workspace/${JOB_NAME}/backend"
                    reuseNode true
                }
            }
            
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
