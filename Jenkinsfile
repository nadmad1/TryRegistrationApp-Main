pipeline {
  agent {label 'Jenkins-Agent'}
  tools {
    jdk 'Java17'
    maven 'Maven'
  }
  environment {
    APP_NAME= "register-app-pipeline"
    RELEASE= "1.0.0"
    DOCKER_USER= "nadmad1"
    DOCKER_PASS= 'Dockerhub'
    IMAGE_NAME= "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG= "${RELEASE}-${BUILD_NUMBER}"
  }
  stages {
    stage("cleanup Workspace") {
      steps {
      deleteDir()
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
    stage("sonarQube Analysis") {
      steps {
        script {
          withSonarQubeEnv(credentialsId: 'jenkins-SonarQube-token') {
            sh "mvn sonar:sonar"
          }
        }
      }
    }
    stage("Quality Gate") {
      steps {
        script {
          waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-SonarQube-token'
        }
      }
    }
    stage("Build & Push Docker Image") {
      steps {
        script {
          docker.withRegistry('',DOCKER_PASS){
            docker_image= docker.build "${IMAGE_NAME}"
          }
          docker.withRegistry('',DOCKER_PASS) {
            docker_image.push("${IMAGE_TAG}")
            docker_image.push('latest')
          }
        }
      }
    }                           
  }
}
