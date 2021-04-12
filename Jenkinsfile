pipeline {
    agent any

    stages {
        
        stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withSonarQubeEnv('SonarQube') { 
                  sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=b3c4408f20988e203ca3912ecced39a1a4399622"
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
