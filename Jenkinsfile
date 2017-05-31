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
        stage('one') {
            steps {
               sh 'chmod 777 ./gradlew'
               sh './gradlew clean creatZip'
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
        stage('two') {
            steps {
                sh "echo integration_tests"
                build job: 'say-hello'
            }
        }
    }
}
