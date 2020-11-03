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
        stage('Build') {
            steps {
		        sh '''
		        mvn -B -DskipTests clean package
		        '''
            }
        }
        stage('Test') {
            steps {
		        sh '''
		        mvn test
		        '''
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
	    stage('SonarQube analysis') {
			steps {
    			withSonarQubeEnv(credentialsId: '6d28e4d2-536d-4a8d-b576-9859374cc6e3', 
								 installationName: 'My SonarQube Server') {
					sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar' 
				}
			}
		}
        stage('Deliver') {
            steps {
		        sh '''
		        ./deliver.sh
		        '''
            }
        }
    }
}
