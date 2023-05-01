pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('unit-test') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").inside {c ->
          sh './src/test.sh'}
        }

      }
    }

    stage('http-test') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").withRun('-p 9005:9000') {c ->
          sh "sleep 5; curl -i http://localhost:9005/test_string"}
        }

      }
    }

    stage('Scan Docker Image for Vulnerabilities') {
      steps {
        script {
          sh 'mkdir -p reports'
          sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'
          def vulnerabilities = sh(script: "trivy image --exit-code 0 --severity HIGH,MEDIUM,LOW --format template --template '@html.tpl' -o reports/image-scan.html --no-progress ${registry}:${env.BUILD_ID}", returnStdout: true).trim()
          echo "Vulnerability Report:\n${vulnerabilities}"
          sh 'trivy image --ignore-unfixed --exit-code 1 --severity CRITICAL --no-progress ${registry}:${BUILD_ID}'
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

    stage('Deploy') {
      steps {
        sh 'docker stop flask-app || true; docker rm flask-app || true; docker run -d --name flask-app -p 9000:9000 armensadoyan/ci-cd:latest'
      }
    }

    stage('Validation') {
      steps {
        sh 'sleep 5; curl -i http://localhost:9000/test_string'
      }
    }

  }
  environment {
    registry = 'armensadoyan/ci-cd-pipeline'
  }
}