pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      agent any
      steps {
        git 'https://github.com/Vijayalaxmi08/WebApp.git'
        script {
          try{
            retry(2){
              bat "start /min stopApp.bat"
            }
          }catch(Exception e){
            echo 'There is no app running in port 9002'
          }
        }

        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKillMe && start /min startApp.bat'
      }
    }

    stage('Test-Automation') {
      parallel {
        stage('QA - UI - Automation') {
          agent any
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
            sleep 10
            bat 'mvn test'
          }
        }

        stage('QA-API-Automation') {
          agent any
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