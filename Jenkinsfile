pipeline {
  agent none
  environment {
    HOME = '/tmp'
  }
  options {
    skipStagesAfterUnstable()
  }
  stages {
    stage('Publish') {
      agent {
        docker {
          image 'circleci/node:10.15.0-stretch-browsers'
          args  '--shm-size="2g"'
        }
      }
      when {
        environment name: 'DEPLOY_STAGE', value: 'RELEASE'
      }
      environment {
        NEXUS_CREDENTIALS = credentials('docker_push')
      }
      steps {
        sh 'echo -n "_auth=" > .npmrc'
        sh 'echo -n "$NEXUS_CREDENTIALS_USR:$NEXUS_CREDENTIALS_PSW" | openssl base64 >> .npmrc'
        sh 'yarn publish --non-interactive'
      }
    }
  }
}
