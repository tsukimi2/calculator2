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
        withCredentials([usernamePassword(credentialsId: 'dockerhubCredential', passwordVariable: 'DOCKER_HUB_PWD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
          dockerId = DOCKER_HUB_USERNAME
          dockerPwd = DOCKER_HUB_PWD
          sh "echo ${dockerPwd} | docker login --username ${dockerId}--password-stdin"
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
