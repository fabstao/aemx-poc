def newImage
pipeline {
agent {
    kubernetes {
      label 'aemxfullalpha'
      yaml """
kind: Pod
metadata:
  name: kaniko
  namespace: jenkins-mvp
spec:
  containers:
  - name: jnlp
    image: jenkins/inbound-agent:latest-jdk11
    workingDir: /home/jenkins
    env:
    - name: GIT_SSL_NO_VERIFY
      value: true
  - name: kaniko
    workingDir: /home/jenkins
    env:
    - name: DOCKER_CONFIG
      value: /kaniko/.docker/
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: harbor-cred
          items:
            - key: .dockerconfigjson
              path: config.json
"""
    }
  }


stages {
/*
 stage("Code Checkout from GitLab") {
  steps {
    container(name: 'jnlp') {
	   git branch: 'dev',
		credentialsId: 'gitlab_access_token',
		url: 'https://gitlab.rax.latamps.tech/fsalaman/aemx-poc-2.git'
    }
  }
 }
 */

/*

   */

   
   stage("Build") {
   steps {
    container(name: 'jnlp') {
       script{
           sh "./mvnw package -Dquarkus.package.type=fast-jar"
           }
      }
    }
   }

   stage('Code Quality Check with SonarQube via MVN') {
   steps {
       script {
           sh "./mvnw sonar:sonar \
                -Dsonar.projectKey=Quarkus-API1 \
                -Dsonar.host.url=http://aemx-sonarqube.sonarqube:9000 \
                -Dsonar.login=fe45b80dfef13f97ece883372e45be37b182d4a9"
           }
       }
   }
   stage('Kaniko - container build') {
   environment {
     PATH = "/busybox:/kaniko:$PATH"
   }
   steps {
     container(name: 'kaniko', shell: '/busybox/sh') {
       script {
           sh "echo Iniciando construcción del container"
           sh "pwd"
           sh "cp src/main/docker/Dockerfile.fast-jar ./Dockerfile"
           sh "ls -l"
           sh '''#!/busybox/sh
                 /kaniko/executor --context=`pwd` --skip-tls-verify --skip-tls-verify-pull --insecure --insecure-pull --insecure-registry --verbosity=debug --destination=harbor.rax.latamps.tech/aemxmvp/quarkusapp:${BUILD_NUMBER}
           '''
           }
      }
     }
   }
}
}
