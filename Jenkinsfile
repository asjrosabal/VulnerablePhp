pipeline {
    agent any

    stages {

        stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withSonarQubeEnv('SonarQube') {
                  sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DevSecOps -Dsonar.sources=. -Dsonar.host.url=http://172.17.0.2:9000 -Dsonar.login=a166eec5c26b59f28c359f18d759756189c8982f"
                }
                }
           }
  }
  
   stage('SCA'){
           steps{
               sh '/var/jenkins_home/dependency-check/bin/dependency-check.sh --project "PHP-SCA" --scan "libs/"'
               archiveArtifacts artifacts: 'dependency-check-report.html', onlyIfSuccessful: true
           }
       }

  stage('Veracode Pipeline Scan') {
  steps {
      script{
          def exists = fileExists 'pipeline-scan.jar'

         if(!exists){
              sh 'curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip'
              sh 'unzip pipeline-scan-LATEST.zip pipeline-scan.jar'
              //
         }
      }

    sh 'java -jar pipeline-scan.jar -vid "2301d001f9edd0be33e11dc05237b2c6" -vkey "f6b0d7718b4033421da833fe3a1d8bf4a222c5cf72949c1fcd4c399c8ac9435d7f7f8c213e1022c818f761981aef705a183c94f4dceec08a1188562d613a7810" -aid 600742 -fs "Very High, High, Medium, Low" -ds "Testing" -r "Jenkins-Labs" -p "PocVeracode"  -f "Archivo.zip" '
  }
}


stage('CleanWorkspace') {
            steps {
                cleanWs()
            }
        }

}
 post {
        cleanup {
            /* clean up our workspace */
            deleteDir()
            /* clean up tmp directory */
            dir("${workspace}@tmp") {
                deleteDir()
            }
            /* clean up script directory */
            dir("${workspace}@script") {
                deleteDir()
            }
        }
    }
}
