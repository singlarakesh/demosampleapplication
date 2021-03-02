pipeline {
   agent any
   tools{
     maven 'Maven3'
     jdk  'java8'
   }
   stages{
     stage('checkout'){
       steps{
       echo 'checkout'
       checkout scm
       }
      }
     stage('Build'){
       steps{
       echo 'Building..'
       bat "mvn clean install"
       }
      }
    stage('sonar analysis'){
       steps{
         withSonarQubeEnv('LocalSonar'){
         bat "mvn sonar:sonar"
         }
       }
      }
      stage('Docker image'){
       steps{
      bat "docker build -t dtr.nagarro.com:443/i-parushasingla-master:${BUILD_NUMBER} . --no-cache -f Dockerfile"
       }
      }
    stage('Docker Push'){
       steps{
        bat "docker push dtr.nagarro.com:443/i-parushasingla-master:${BUILD_NUMBER}"
       }
      }
     stage('Run image'){
       steps{
        bat "docker -d --name c-parushasingla-master -p 8080:8080 dtr.nagarro.com:443/i-parushasingla-master:${BUILD_NUMBER}"
       }
      }
   }
}