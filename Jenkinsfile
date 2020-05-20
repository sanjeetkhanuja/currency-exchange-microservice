pipeline {
	agent any 
	
	//{ 
	//	docker { 
	//		image 'maven:3.6.3' 
	//	} 
	//}
	
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout') {
			steps {
				echo "Checkout"
				sh 'mvn --version'
				sh 'docker --version'
				echo "Path : $PATH"
				echo "Build Number: $env.BUILD_NUMBER"
				echo "Job Name: $env.JOB_NAME"
				echo "Build Tag: $env.BUILD_TAG"
				echo "Build URL: $env.BUILD_URL"
			}
		}
		stage('Compile') {
			steps {
				echo "Compile"
				sh "mvn clean compile"
			}	
		}
		stage('Test') {
			steps {
				echo "Test"
				sh "mvn test"
			}	
		}
		stage('Integration Test') {
			steps {
				echo "Test"
				sh "mvn failsafe:integration-test failsafe:verify"
			}	
		}
		stage('Package') {
			steps {
				echo "Package"
				sh "mvn package -DskipTests"
			}	
		}
		stage('Build Docker image') {
			steps {
				script {
					dockerImage = docker.build ("sanjeetkhanuja/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		
		stage('Push Docker image') {
			steps {
				script {
					docker.withRegistry('', 'dockerHub') {
						dockerImage.push();
						dockerImage.push('latest');
					}	
				}
			}
		}
	} 
	post {
		always {
			echo "always"
		}
		success {
			echo "success"
		}
		failure {
			echo "failure"
		}
	}
}
