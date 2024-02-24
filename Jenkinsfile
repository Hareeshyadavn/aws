pipeline {
    agent { label 'jenins-agent'}
    tools {
        jdk 'Java17'
        maven 'Maven3'
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
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                    sh "mvn test"
                    }
                }
                
            }
        }
    }
}
