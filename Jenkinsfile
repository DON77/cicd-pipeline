pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('Build Application') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").withRun('-p 3001:3000') {c ->
          sh ''' sleep 5; chmod +x /scripts/build.sh
./scripts/build.sh'''}
        }

      }
    }

    stage('test') {
      steps {
        script {
          docker.image('node:12').withRun('-p 3001:3000') {c ->
          sh 'sleep 4; cd scripts/ && chmod +x test.sh && ./test.sh'}
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