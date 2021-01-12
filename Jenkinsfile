
pipeline {
    agent any

    environment {
        DEPENDENCIES = "./app/build/dependencies/debug"
        IQ_SCAN_URL = ""
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'gradlew clean copyDependenciesDebug assembleDebug'
            }
        }
	    
        stage('Nexus IQ Scan'){
            steps {
                script{
                    dir("${DEPENDENCIES}") {
			    try {
				def policyEvaluation = nexusPolicyEvaluation enableDebugLogging: false, 
								  failBuildOnNetworkError: true, 
								  iqApplication: selectedApplication('mobi-app-ci'), 
								  iqScanPatterns: [[scanPattern: '*']], 
								  iqStage: 'build', 
								  jobCredentialsId: 'admin'	    
				    
				echo "Nexus IQ scan succeeded"
				IQ_SCAN_URL = "${policyEvaluation.applicationCompositionReportUrl}"
			    } 
			    catch (error) {
				def policyEvaluation = error.policyEvaluation
				echo "Nexus IQ failed... scan vulnerabilities detected"
				throw error
			    }
		    }
                }
            }
        }

    }
}

