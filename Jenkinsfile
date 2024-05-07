pipeline {

	agent any
	tools{
		jdk 'openJDK21'
		
	}
	environment{
		APP_NAME = "senipy"
		RELEASE = "1.0.0"
		DOCKER_USER = "senipy"
		DOCKER_PASS = 'dockerhub'
		IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
		IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}


	triggers{
		pollSCM('H/2 * * * *')
	}

	stages{
		stage("Clean Workspace"){
			steps{
			cleanWs()
			
			}
		}
		
		stage("Checkout from SCM"){
			steps{
				git branch: 'main', credentialsId: 'github', url: 'https://github.com/RAKSHITH13IGNATIUS/newrepo-.git'
			}					
		}
	
		stage("Build and push Docker image"){
                        steps{
                                script{
					docker.withRegistry('',DOCKER_PASS){
					docker_image = docker.build "${IMAGE_NAME}"			
					}
					docker.withRegistry('',DOCKER_PASS){
                                        docker_image.push("${IMAGE_TAG}") 
					docker_image.push('latest')
					}
                        	}

			}
		}
		stage("Docker logout"){
			steps{
				script{
				docker.withRegistry('',DOCKER_PASS){
					sh 'docker logout'
				}
				}
			}
		}
}
}
