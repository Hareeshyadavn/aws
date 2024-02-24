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
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Hareeshyadavn/Hareeshyadavn.git'
            }
        }
        stage ("Build Application"){
            steps {
                sh '''sudo cd /CICD
                mvn clean package '''
            }
        }
        stage ("Test Application"){
            steps{
                sh "mvn test"
            }
        }
        
    }
}
