pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
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
          def phase = isReleasingBranch() ? 'deploy' : 'verify'
          maven cmd: "clean ${phase}"
        }
	withChecks('Maven Issues') {
          recordIssues tools: [mavenConsole()], qualityGates: [[threshold: 1, type: 'TOTAL']]
        }
        archiveArtifacts 'target/*.zip'
      }
    }
  }
}
