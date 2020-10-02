pipeline {
  environment {
    registry = 'osiris65/calculator'
    registryCredential = 'dockerhubCredential'
    dockerId = ''
    dockerPwd = ''
    dockerImage = ''
  }

  agent any

  stages {
    stage("Compile") {
      steps {
        sh "./gradlew compileJava"
      }
    }
    stage("Unit test") {
      steps {
        sh "./gradlew test"
      }
    }
    stage("Package") {
      steps {
        sh "./gradlew build"
      }
    }
    stage("Docker build") {
      steps {
        script {
          dockerImage = docker.build registry
        }
      }
    }
    stage("Docker push") {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage("Deploy to staging") {
      steps {
        sh "docker run -d --rm -p 8765:8080 --name calculator osiris65/calculator"
      }
    }
    stage("Acceptance test") {
      steps {
        sleep 60
        sh "chmod +x acceptance_test.sh && ./acceptance_test.sh" 
      }
    }
  }

  post {
    always {
      sh "docker stop calculator"
      sh "docker container rm calculator"
      sh "docker rmi $registry"
    }
  }
}
