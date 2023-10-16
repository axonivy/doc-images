pipeline {
  agent {
    docker {
      image 'maven:3.9.4-eclipse-temurin-21'
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
          def phase = isReleaseOrMasterBranch() ? 'deploy' : 'verify'
          maven cmd: "-Duser.home=/var/maven clean ${phase}"
        }
        archiveArtifacts 'target/*.zip'
      }
    }
  }
}

def isReleaseOrMasterBranch() {
  return env.BRANCH_NAME == 'master' || env.BRANCH_NAME.startsWith('release/') 
}
