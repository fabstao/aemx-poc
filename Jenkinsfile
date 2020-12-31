pipeline {
agent { label 'slavejdk11' }
stages {
/*
 stage("Code Checkout from GitLab") {
  steps {
   git branch: 'dev',
    credentialsId: 'gitlab_access_token',
    url: 'https://gitlab.rax.latamps.tech/fsalaman/aemxpoc-jdk-pipeline.git'
  }
 }
 */
 /*
   stage('Code Quality Check via SonarQube') {
   steps {
       script {
       def scannerHome = tool 'scanner';
           withSonarQubeEnv("SonarScanner") {
           sh "${tool("scanner")}/bin/sonar-scanner \
           -Dsonar.projectKey=Quarkus-API1 \
           -Dsonar.sources=. \
           #-Dsonar.css.node=. \
           -Dsonar.host.url=http://aemx-sonarqube.sonarqube:9000 \
           -Dsonar.login=fe45b80dfef13f97ece883372e45be37b182d4a9"
               }
           }
       }
   }
*/


   stage('Code Quality Check with SonarQube via MVN') {
   steps {
       script {
       def scannerHome = tool 'scanner';
           withSonarQubeEnv("SonarScanner") {
           sh "mvn sonar:sonar \
                -Dsonar.projectKey=Quarkus-API1 \
                -Dsonar.host.url=http://aemx-sonarqube.sonarqube:9000 \
                -Dsonar.login=fe45b80dfef13f97ece883372e45be37b182d4a9"
               }
           }
       }
   }

   
   stage("Build") {
   steps {
       script{
           sh "./mvnw compile quarkus:dev"
           }
       }
   }
}
}