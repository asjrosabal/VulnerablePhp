pipeline {
    agent any

    stages {

      stage('Build Code') {
          steps {
              deleteDir()

          }
      }

        stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withSonarQubeEnv('SonarQube') {
                  sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DevSecOps -Dsonar.sources=. -Dsonar.host.url=http://172.17.0.3:9000 -Dsonar.login=b0c43a0794e31d3af8b91462bd11feb3e65e43d2"
                }
                }
           }
        }

        stage('Veracode Pipeline Scan') {
          steps {
              script{
                  def exists = fileExists 'pipeline-scan.jar'

                 if(!exists){
                      sh 'curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip'
                      sh 'unzip pipeline-scan-LATEST.zip pipeline-scan.jar'
                 }
              }

            sh 'java -jar pipeline-scan.jar -vid "2301d001f9edd0be33e11dc05237b2c6" -vkey "f6b0d7718b4033421da833fe3a1d8bf4a222c5cf72949c1fcd4c399c8ac9435d7f7f8c213e1022c818f761981aef705a183c94f4dceec08a1188562d613a7810" -aid 600742 -ds "Testing" -r "Jenkins-Labs" -p "PocVeracode" -fs "Very High, High, Medium, Low" -f "Archivo.zip" '
          }
      }

      stage('Units Test') {
          steps {
              echo "=== Starting Units Test ==="
          }
      }

      stage('Coverage Test') {
          steps {
                echo "=== Starting Coverage Test ==="
          }
      }

      stage('Docker Image') {
          steps {
              echo "=== Creating Docker Image ==="
          }
      }

      stage('Kubernetes Deploy') {
          steps {
              echo "=== Deploying Kuberntes ==="
          }
      }

      stage('cleanup') {
          steps {
              deleteDir()

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
