pipeline {
  agent any
  stages {
    stage('') {
      steps {
        build './gradlew clean createZip'
      }
    }
  }
  environment {
    DEV_HOST = '172.30.3.154'
  }
}