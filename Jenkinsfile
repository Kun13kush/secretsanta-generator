pipeline {
    agent any
    
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    
    environment{
        
        SCANNER_HOME= tool 'sonar-scanner'
    } 

    stages {
        stage('Git Checkout') {
            steps {
               git 'https://github.com/Kun13kush/secretsanta-generator.git'
            }
        }
        stage('Compile') {
            steps {
               sh "mvn compile"
            }
        }
        stage('Test') {
            steps {
               sh "mvn test"
            }
        }
        stage('Sonarqube analysis') {
            steps {
				withSonarQubeEnv(credentialsId: 'sonar') {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=santa \
					-Dsonar.projectKey=santa -Dsonar.java.binaries=. '''
               }
            }
		}
		stage('Owasp scan') {
            steps {
               dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
			}
		}
		stage('build application') {
            steps {
				sh "mvn package"
			}
		}
	}
}     
