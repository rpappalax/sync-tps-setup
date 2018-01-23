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
      environment {
        TEST_ENV = "${TEST_ENV ?: JOB_NAME.split('\\.')[1]}"
        //RECIPIENT = "${RECIPIENT}"
	SYNC_TPS_CONFIG_STAGE = credentials('SYNC_TPS_CONFIG_STAGE')
	SYNC_TPS_CONFIG_PROD = credentials('SYNC_TPS_CONFIG_PROD')
      }
      steps {
        sh "echo ${env.SYNC_TPS_CONFIG_STAGE}"
        sh ". /tests/venv/bin/activate"
        sh "/tests/run ${env.TEST_ENV} ${env.SYNC_TPS_CONFIG_STAGE}"
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

