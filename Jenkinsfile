pipeline {
  agent any
  
  stages {
      stage('Build Artifact') {
            steps {
              
              sh "mvn clean package -DskipTests=true" 
              archive 'target/*.jar'
              
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
             
              sh "mvn test"
              
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
        

       stage('Sonarqube Analysis - SAST') {
            steps {
             
              withSonarQubeEnv('sonarcloud'){
              sh "mvn sonar:sonar \
                              -Dsonar.projectKey=karthik0741_newmaven \
                        -Dsonar.host.url=https://sonarcloud.io" 
                          
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        }
     }
}
