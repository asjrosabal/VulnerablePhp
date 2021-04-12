pipeline {
    agent any

    stages {
        
        stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withSonarQubeEnv('SonarQube') { 
                  sh "${scannerHome}/bin/sonar-scanner  -Dsonar.projectKey=DevSecOps -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=7d49693b8b841c611a35a3393d3748863bccbc9b"
                }
                }
           }
  }
        
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
