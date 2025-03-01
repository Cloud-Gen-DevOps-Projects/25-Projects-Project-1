pipeline{
	agent any 
	tools{
		jdk "jdk17"
		maven "maven3"
	}
	environment{
		SCANNER_HOME= tool "sonar-task"
		WAR_FILE= '/var/lib/jenkins/workspace/Project-1/webapp/target/*.war'
		JOB_DIR = "${env.JOB_NAME}"
		TARGET_DIR = "/var/lib/jenkins/workspace/${JOB_NAME}"
	}
	stages{
		stage("Code Checkout"){
			steps{
				git branch: 'main', url: 'https://github.com/Cloud-Gen-DevOps-Projects/25-Projects-Project-1.git'
			}
		}
		
		stage("Code Compile"){
			steps{
				sh "mvn compile"
			}
		}
		
		stage("Trivy FS"){
			steps{
				sh "trivy fs . --format table -o fs.html"
			}
		}
			
		stage("Code Analysis"){
			steps{
				withSonarQubeEnv('sonar-token') {
					sh "mvn clean install sonar:sonar"
 					}
			}
		}
		stage("Artifact Build"){
			steps{
				sh "mvn package"
			}
		}
		
		stage("Artifact Deploy"){
			steps{
				sh "mvn deploy"
			}
		}
		
		stage("Copy Artifact"){
			steps{
				script{
			sh " cp ${WAR_FILE} ${TARGET_DIR}/"
			echo "War File Copied to ${TARGET_DIR}"
			}
		}
	}
		stage("Docker Image Build"){
			steps{
				script{
				sh "docker build -t cloudgen01/registration-app:latest ."
				}
				}
				}
	
		stage("Docker image Push"){
			steps{
			script{
			withDockerRegistry(credentialsId: 'Docker-Token', url: 'https://index.docker.io/v1/') {
				sh "docker push cloudgen01/registration-app:latest"
						}
					}
				}
			}
			
	
		
		
		
		
		
	}
}

				
