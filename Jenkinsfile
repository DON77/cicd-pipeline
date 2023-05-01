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
           //docker.image("${registry}:${env.BUILD_ID}").inside {c ->
          sh 'ls -a && cd scripts/ && ls -lah build.sh  && sudo .build.sh'
        }

      }
    }

    stage('test') {
      steps {
        script {
          //docker.image("${registry}:${env.BUILD_ID}").inside {c ->
            sh './scripts/test.sh'
        }
      }
    }

    stage('Docker image build') {
      steps {
        script {
         // true
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
