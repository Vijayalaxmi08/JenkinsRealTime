pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      steps {
        git 'https://github.com/Vijayalaxmi08/WebApp.git'
        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKillMe && start /min startApp.bat'
      }
    }

    stage('Test-Automation') {
      parallel {
        stage('QA - UI - Automation') {
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
            sleep 10
            bat 'mvn test'
          }
        }

        stage('QA-API-Automation') {
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppApiAutomation.git'
            sleep 10
            bat 'mvn test'
          }
        }

      }
    }

  }
}