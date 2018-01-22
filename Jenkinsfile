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
        sh 'echo ${env.SYNC_TPS_CONFIG_STAGE}'
        sh '/tests/venv/bin/activate'
        sh '/tests/run "${env.TEST_ENV}" "${env.SYNC_TPS_CONFIG_STAGE}"'
      }
    }
  }
  post {
    success {
      emailext(
        body: 'TPS "$TEST_ENV" success!\n\n"$BUILD_URL"',
        replyTo: '$DEFAULT_REPLYTO',
        subject: 'TPS $TEST_ENV Success',
        to: '$DEFAULT_RECIPIENTS')
    }
    failure {
      emailext(
        attachLog: true,
        body: 'TPS "$TEST_ENV" failure\n\n"$BUILD_URL"',
        replyTo: '$DEFAULT_REPLYTO',
        subject: 'TPS $TEST_ENV Failure',
        to: '$DEFAULT_RECIPIENTS')
    }
    changed {
      ircNotification()
    }
  }
}

