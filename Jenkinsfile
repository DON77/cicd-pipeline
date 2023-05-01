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
          docker.image('node:12').inside {c ->
          sh '''
cd scripts/ && chmod +x build.sh && ./build.sh
'''}
        }

      }
    }

    stage('test') {
      steps {
        script {
          docker.image('node:12').inside {c ->
          sh 'cd scripts/ && chmod +x test.sh && ./test.sh'}
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