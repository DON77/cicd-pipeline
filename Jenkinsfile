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
          sh '''
          curl -fsSL https://deb.nodesource.com/setup_14.x 
          apt-get install -y nodejs
          cd scripts/ && chmod +x build.sh && ./build.sh
          '''
        }

      }
    }

    stage('test') {
      steps {
        script {
          sh 'cd scripts/ && chmod +x build.sh && ./test.sh'
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
