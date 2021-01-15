
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'gradlew clean build'
            }
            post {
                success {
                    echo "Now scanning with Nexus IQ"
                    sh 'gradlew NexusIQScan'
                }
            }
        }
    }
}

