pipeline {
  agent {label 'Jenkins-Agent'}
  tools {
    jdk 'java17'
    maven 'maven3'
  }
  stages {
    stage("cleanup Workspace") {
      steps {
      cleanWS()
      }
    }
    stage("checkout from scm") {
      steps {
      git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/nadmad1/TryRegistrationApp-Main.git'
      }
    }
    stage("Build Application") {
      steps {
      sh "mvn clean package"
      }
    }
    stage("Test Application") {
      steps {
      sh "mvn test"
      }
    }
  }
}
