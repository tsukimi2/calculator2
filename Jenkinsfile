pipeline {
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
        sh "docker build -t osiris65/calculator ."
      }
    }
    stage("Docker push") {
      steps {
        sh "docker push osiris65/calculator"
      }
    }
    stage("Deploy to staging") {
      steps {
        sh "docker run -d --rm 8765:8080 --name calculator osiris65/calculator"
      }
    }
  }
}
