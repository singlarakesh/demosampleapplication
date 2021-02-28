pipeline {
    agent any
    tools
    {
    	maven 'MAVEN_HOME'
    	jdk 'JAVA_HOME'
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
           withSonarQubeEnv("Test_Sonar")
           {
             bat "mvn sonar:sonar"
           }
         }
       }     
 
    }  
}
