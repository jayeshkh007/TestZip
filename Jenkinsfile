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
         }
        stage('two') {
            steps {
                sh "echo integration_tests"
                build job: 'say-hello'
            }
        }
    }
}
