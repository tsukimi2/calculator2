pipeline {
  agent any

  stages {
    stage("Compile") {
      steps {
        sh "./gradlew compileJava"
      }
    }
    stage("Unit test") {
      sh "./gradlew test"
    }
  }
}
