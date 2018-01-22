pipeline {
  agent {
    dockerfile true
  }
  libraries {
    lib('fxtest@1.9')
  }
  options {
    ansiColor('xterm')
    timestamps()
    timeout(time: 1, unit: 'HOURS')
  }
  stages {
    stage('Test') {
      steps {
        sh "echo ${env.SYNC_TPS_CONFIG_STAGE}"
        sh "echo whoami"
        sh "/tests/venv/bin/activate"
        sh '/tests/run "${env.TEST_ENV}" "${env.SYNC_TPS_CONFIG_STAGE}"'
      }
    }
  }
  post {
    failure {
      emailext(
        attachLog: true,
        body: '$BUILD_URL\n\n$FAILED_TESTS',
        replyTo: '$DEFAULT_REPLYTO',
        subject: '$DEFAULT_SUBJECT',
        to: '$DEFAULT_RECIPIENTS')
    }
    changed {
      ircNotification()
    }
  }
}

