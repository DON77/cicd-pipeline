pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          sh "./scripts/build.sh"
        }

      }
    }

    stage('test') {
      steps {
        script {
          sh "./scripts/test.sh"
        }

      }
    }

    stage('Docker image build') {
      steps {
        script {
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('Publish') {
      steps {
        script {
          docker.withRegistry('', 'docker-id-ci') {
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
          }
        }

      }
    }

  }
  environment {
    registry = 'armensadoyan/ci-cd-pipeline'
  }
}