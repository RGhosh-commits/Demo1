pipeline {
agent any
tools{
maven "Maven"
}
environment{
sonarscanner = tool 'SonarScanner'
}
stages{
stage("build-test-deploy"){
steps{
sh 'mvn clean package'
      }
                          }
    stage('SonarQube Analysis'){
            steps{
               withSonarQubeEnv('sonarqube'){
                     sh '${sonarscanner}/bin/sonar-scanner -Dproject.settings=./Sonar.properties'
                }
            }
        }
   stage("Quality Gate") {
            steps {
              timeout(time: 3, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
  stage('Pushing to Nexus'){
            steps{
               withCredentials([usernamePassword(credentialsId: 'nexus-credentialss', passwordVariable: 'password', usernameVariable: 'username'),string(credentialsId: 'NEXUS_URL', variable: 'nexus_url')]){
                    //sh 'curl -u ${username}:${password} --upload-file target/BMIBeta-${BUILD_NUMBER}.jar http://18.224.155.110:8081/nexus/content/repositories/devopstraining/comrades/BMI-${BUILD_NUMBER}.war'
                   sh 'curl -v -F file=@target/ADD-0.jar -u ${username}:${password} http://${nexus_url}/nexus/content/repositories/devopstraining/comrades/bmi/BMI/BMI-${BUILD_NUMBER}.war'
               }
            }
        }
  
}
}

