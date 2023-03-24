
//Declarative
pipeline {
	agent any
	// agent { docker { image 'maven:3.6.3'} }
	// agent { docker { image 'node:13.8'} }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout') {
			steps{
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "Path - $PATH"
				echo "BUILD NUMBER - $env.BUILD_NUMBER"
				echo "BUILD ID - $env.BUILD_ID"
				echo "JOB NAME - $env.JOB_NAME"
				echo "BUILD TAG - $env.BUILD_TAG"
				echo "BUILD URL - $env.BUILD_URL"
			}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps{
				sh "mvn test"
			}
		}
		stage('Integration Test') {
			steps{
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package') {
			steps{
				sh "mvn packege -DskipTests"
			}
		}
		stage('Build Docker Image') {
			steps {
				// "docker build -t skytechbv/curency-exchange-devops:$env.BUILD_TAG ."
				script {
					dockerImage = docker.build("skytechbv/curency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Push Docker Image') {
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
			echo "I'm awesome. I run always"
		}
		success {
			echo "I run only when you are successful!"
		}
		failure {
			echo "I run only when you are fail!"
		}
	}
}
