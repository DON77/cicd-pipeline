pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        script {
          checkout scm
        }

      }
    }

    stage('Build Application') {
      steps {
        script {
          sh 'ls -a && cd scripts/ && ls -lah build.sh  && chmod +r build.sh && .build.sh'
        }

      }
    }

    stage('test') {
      steps {
        script {
          sh './scripts/test.sh'
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