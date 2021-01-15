
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'gradlew clean build'
            }
        }

        stage('Nexus IQ Scan') {
             steps {
                sh 'gradlew nexusIQScan'
             }
         }
    }
}

