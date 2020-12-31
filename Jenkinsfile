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
   stage('Code Quality Check via SonarQube') {
   steps {
       script {
       def scannerHome = tool 'scanner';
           withSonarQubeEnv("SonarScanner") {
           sh "${tool("scanner")}/bin/sonar-scanner \
           -Dsonar.projectKey=test-node-js \
           -Dsonar.sources=. \
           -Dsonar.css.node=. \
           -Dsonar.host.url=http://aemx-sonarqube.sonarqube:9000 \
           -Dsonar.login=your-generated-token-from-sonarqube-container"
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