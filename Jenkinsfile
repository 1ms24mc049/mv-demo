
pipeline {
	agent any
	
	environment {
		DOCKERHUB_CRED=credentials('dockerhub')
		IMAGE_NAME="1ms24mc049/mv-demo"
	}
	
	stages {
		stage('checkout') {
			steps {
				git url:'git@github.com:1ms24mc049/mv-demo.git', branch:'main'
			}
		}

		stage('Build Maven Project') {
			steps {
				sh "mvn clean package -DskipTests"
			}
		}
		
		stage('Build Docker Image') {
            		steps {
                		script {
                    			dockerImage = docker.build("${IMAGE_NAME}:latest")
                		}
            		}
        	}

		stage('Push Docker Image'){
			steps {
				script {
					docker.withRegistry('https://index.docker.io/v1/','dockerhub'){
						dockerImage.push()
					}
				}
			}
		}	
	}
	
	post {
		success {
			echo "Pipeline Successful"
		}
		failure {
			echo "Pipeline Failure"
		}
		always {
			echo "Cleaning workspace"
			deleteDir()
		}
	}

}
