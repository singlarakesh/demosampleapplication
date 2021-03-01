pipeline {
    agent any
    tools
    {
    	maven 'Maven3'
    	jdk 'java8'
    }
    stages {
       stage('Checkout'){
          steps {
             echo 'build in from master branch'
              checkout scm
          }
       }
       stage('Build'){
          steps{
            echo 'building..'
            bat "mvn clean install"
          }
       }
       stage('Unit Testing'){
          steps{
            echo 'testing..'
            bat "mvn test"
          }
       }
       stage('Sonar Analysis'){
        steps{
           echo 'Sonar Analysis..'
           withSonarQubeEnv("LocalSonar")
           {
             bat "mvn sonar:sonar"
           }
         }
       }    
          stage('Docker Image'){
        steps{
          bat "docker build -t dtr.nagarro.com:443/i-parushasingla-master:${BUILD_NUMBER} . --no-cache -f Dockerfile"
        }
      }
 
    }  
}
