pipeline {
	agent any 
	
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'mymvn'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout') {
			steps {
				echo "Checkout"
				bat 'mvn --version'
				bat 'docker --version'
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
				bat "mvn clean compile"
			}	
		}
		/*
		stage('Test') {
			steps {
				echo "Test"
				bat "mvn test"
			}	
		}
		stage('Integration Test') {
			steps {
				echo "Test"
				bat "mvn failsafe:integration-test failsafe:verify"
			}	
		}
		*/
		stage('Package') {
			steps {
				echo "Package"
				bat "mvn package -DskipTests"
			}	
		}
		stage('Build Docker image') {
			steps {
				script {
					echo "Build Docker image"
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
