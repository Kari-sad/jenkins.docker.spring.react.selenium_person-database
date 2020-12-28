pipeline {
	agent none
	stages {
		stage('Run') {
			parallel {
				stage('Back-end') {
					agent {
						docker {
							image 'jamesdbloom/docker-java8-maven:latest' 
							args '-v $HOME/.m2:/root/.m2 -p 8079:8079'
						}
					}
					stages {
						stage('Set Up') {
							steps {
								script {
									sh 'rm -rf spring'
								}
							}
						}
						stage('SCM Checkout') {
							steps {
								sh 'git clone https://github.com/Kari-sad/jenkins.docker.spring.react.selenium_person-database $PWD/spring'
							}
						}
						stage('Compile-Package-Test') {
							steps {
								script {
									dir('$PWD/spring/webservice') {
										sh "mvn spring-boot:run"
									}
								}
							}
						}
					}
				}
				stage('Front-end') {
					agent {
						docker {
							image 'node:10' 
							args '-v $HOME/.m2:/root/.m2 -p  8078:8078'
						}
					}
					stages {
						stage('Set Up') {
							steps {
								script {
									sh 'rm -rf react'
								}
							}
						}
						stage('SCM Checkout') {
							steps {
								sh 'git clone https://github.com/Kari-sad/jenkins.docker.spring.react.selenium_person-database $PWD/react'
							}
						}
						stage('Compile-Package-Test') {
							steps {
								script {
									dir('$PWD/react/client') {
										sh "npm install"
										sh "npm start"
									}
								}
							}
							
					
						}
					}
				}
				
				stage('Integration-Testing') {
					agent {
						docker {
							image 'jamesdbloom/docker-java8-maven:latest'
							args '-v /root/.m2:/root/.m2 -p 8050:8050'
						}
					}
					stages {
						stage('Set Up') {
							steps {
								script {
									sh 'rm -rf testing'
								}
							}
						}
						stage('SCM Checkout') {
							steps {
								script {
									sh 'git clone https://github.com/Kari-sad/jenkins.docker.spring.react.selenium_person-database $PWD/testing'
								}
							}
						}
						stage('Compile-Package-Test') {
							steps {
								script {
									dir('$PWD/testing/integration-testing') {
										sh "mvn package -Dmaven.test.failure.ignore=true"
									}
								}
							}
						}
					}
				}		
			}
		}
	}
}