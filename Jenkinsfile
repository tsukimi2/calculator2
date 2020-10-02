pipeline {
  environment {
    registry = 'osiris65/calculator'
    registryCredential = 'dockerhubCredential'
    dockerId = ''
    dockerPwd = ''
  }

  def dockerImage = ''

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
        sh "docker run -d --rm 8765:8080 --name calculator osiris65/calculator"
      }
    }
    stage("Acceptance test") {
      steps {
        sleep 60
        sh "./acceptance_test.sh" 
      }
    }
  }

  post {
    always {
      sh "docker stop calculator"
      sh "docker rmi $registry"
    }
  }
}
