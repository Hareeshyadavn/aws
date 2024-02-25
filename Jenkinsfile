pipeline {
    agent { label 'jenins-agent'}
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
	        APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "yadavn1692"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	        
    }
    stages{
        stage ("cleanup workspace"){
            steps {
                cleanWs()
            }
        }
        stage ("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Hareeshyadavn/aws.git'
            }
        }
        stage ("Build Application"){
            steps {
                sh '''pwd
                mvn clean package '''
            }
        }
        stage ("Test Application"){
            steps{
                sh "mvn test"
            }
        }
        stage ("sonarqube analysis"){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins_sonar'){
                    sh "mvn sonar:sonar"
                    }
                }
                
            }
        }
        stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonar'
                }	
            }
        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

       }
        
    }
}
