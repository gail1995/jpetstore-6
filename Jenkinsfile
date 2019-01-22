pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'starting build'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
    stage('Testing') {
      parallel {
        stage('Sonar Test') {
          steps {
            sh 'mvn sonar:sonar -Dsonar.host.url=http://13.229.217.24:8081 -Dlicense.skip=true'
          }
        }
        stage('Print tester credential') {
          steps {
            echo "The tester us ${TESTER}"
          }
        }
        stage('Print Build Number') {
          steps {
            echo "This is build number ${BUILD_ID}"
          }
        }
      }
    }
    stage('Jfrog push') {
      steps {
        echo 'print jfrog'
        script {
          def server = Artifactory.server "artifactory"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean install -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

      }
    }
    stage('Deploy prompt') {
      steps {
        input 'Deploy production'
      }
    }
    stage('Deployment') {
      steps {
        echo 'Deployment'
      }
    }
  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'LIMDYNASTY'
  }
}