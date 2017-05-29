#!groovy

pipeline {
  agent any

  environment {
    DEV_HOST = "172.30.3.114"
    }

  options {
    buildDiscarder(logRotator(numToKeepStr: '20'))
    disableConcurrentBuilds()
  }

  parameters {
    string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: 'deployment environment')
    booleanParam(name: 'URGENT_BUILD', defaultValue: false, description: 'is this is an urgent build and can not wait')
  }

  stages {
     stage('Build') {
      steps {
        sh 'chmod +x ./gradlew'
        sh './gradlew clean createZip'
        //archiveArtifacts artifacts: '**/build/distributions/*.zip', fingerprint: true
      }

      post {
        always {
          junit '**/build/test-results/test/*.xml'
          pmd pattern: '', canComputeNew: false, healthy: '', unHealthy: ''
          checkstyle pattern: 'build/reports/checkstyle/*.xml', canComputeNew: false, healthy: '', unHealthy: ''
          step([$class: 'JacocoPublisher'])
          step([$class: 'AnalysisPublisher', canComputeNew: false, defaultEncoding: '', healthy: '', unHealthy: ''])
          step([$class: 'GitHubCommitStatusSetter'])
        }
      }
    }

  stage('Integration Tests') { // selenium test cases
      steps {
        sh "echo integration_tests"
        //job ''
      }
    }

  }

}
