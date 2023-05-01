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
          docker.image('node:7.8').inside {c ->
          sh '''chmod +x scripts/build.sh

cp package.json /opt/ cd /opt./scripts/build.sh
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