pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          ./build.sh
         
        }

      }
    }

    stage('test') {
      steps {
        script {
         
          ./test.sh
        
        }

      }
    }
  stage('Docker image build'){
    steps{
      script{
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

//     stage('Deploy') {
//       steps {
//         sh 'docker stop flask-app || true; docker rm flask-app || true; docker run -d --name flask-app -p 9000:9000 armensadoyan/ci-cd:latest'
//       }
//     }

//     stage('Validation') {
//       steps {
//         sh 'sleep 5; curl -i http://localhost:9000/test_string'
//       }
//     }
   }
  environment {
    registry = 'armensadoyan/ci-cd-pipeline'
  }
}
