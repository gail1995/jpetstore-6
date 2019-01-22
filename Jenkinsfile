pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'starting build'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
  }
  tools {
    maven 'maven'
  }
}