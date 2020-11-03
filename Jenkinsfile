@Library('common-jk-build')_

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
    			withSonarQubeEnv(credentialsId: '021ef3b2-af35-45b6-9eeb-adaa10aa3971', 
								 installationName: 'My SonarQube Server') {
					sh 'mvn http://devtest-sonar1.fyre.ibm.com:9000:sonar-maven-plugin:3.7.0.1746:sonar' 
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
