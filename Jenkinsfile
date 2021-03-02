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
      stage('Docker stop running container'){
       steps{
        bat """
        docker ps -aqf name=c-parushasingla-master>temp.txt
        set /p comd1=<tmp.txt
        if[%comd1%]==[](echo "no running container")
        else(
        docker stop %comd1%
        docker rm -f %comd1%
        )
        """
       }
      }
     stage('Run image'){
       steps{
        bat "docker run -d --name c-parushasingla-master -p 9090:8080 dtr.nagarro.com:443/i-parushasingla-master:${BUILD_NUMBER}"
       }
      }
   }
}
