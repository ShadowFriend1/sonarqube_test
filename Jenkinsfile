pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2 -u root:root'
        }
    }
    stages {
	    stage('Check Versions') {
	        steps {
		        sh '''
		        mvn --version
		        '''
	        }
	    }
		stage('SonarQube analysis') {
			environment {
        		scannerHome = tool 'SonarQubeScanner'
    		}    
			steps {
        		withSonarQubeEnv('sonarqube') {
            		sh "${scannerHome}/bin/sonar-scanner"
        	}
		}
    }
}
