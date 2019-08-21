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
            
                sh 'mvn test'
           
                sh './jenkins/scripts/deliver.sh' 
                
                sh 'mkdir -p ../frontend'
                
                sh 'cp ./target/*.jar ../frontend/'
            }
            
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
