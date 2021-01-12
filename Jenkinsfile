
pipeline {
    agent any

    environment {
        DEPENDENCIES = "${WORKSPACE}/app/build/dependencies/debug"
        IQ_SCAN_URL = ""
    }
    
    stages {
        stage('Build') {
            steps {
                sh './gradlew clean copyDependenciesDebug assembleDebug'
            }
        }

        stage('Nexus IQ Scan'){
			steps {
				script {
            	    nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: selectedApplication('mobi-app-ci'), iqScanPatterns: [[scanPattern: '${DEPENDENCIES}']], iqStage: 'build', jobCredentialsId: 'admin'
				}
			}
		}

        stage('NexusIQ Scan'){
            steps {
                script{
                    try {
                        def policyEvaluation = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: selectedApplication('mobi-app-ci'), iqScanPatterns: [[scanPattern: '${DEPENDENCIES}']], iqStage: 'build', jobCredentialsId: 'admin'
                        echo "Nexus IQ scan succeeded: ${policyEvaluation.applicationCompositionReportUrl}"
                        IQ_SCAN_URL = "${policyEvaluation.applicationCompositionReportUrl}"
                    } 
                    catch (error) {
                        def policyEvaluation = error.policyEvaluation
                        echo "Nexus IQ scan vulnerabilities detected', ${policyEvaluation.applicationCompositionReportUrl}"
                        throw error
                    }
                }
            }
        }

    }
}

