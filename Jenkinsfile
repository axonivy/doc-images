pipeline {
  agent {
    docker {
      image 'maven:3.5.2-jdk-8'
    }
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '3'))
  }

  triggers {
    cron '@midnight'
  }

  stages {    
    stage('build') {
      steps {
        script {
          maven cmd: "clean deploy"
        }
        archiveArtifacts 'target/*.zip'
      }
    }
  }
}
