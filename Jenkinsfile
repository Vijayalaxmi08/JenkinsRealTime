pipeline {
  agent any
  tools {
        maven "Maven"
    }
  stages {
    stage('Dev-Build') {
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

    stage('Test Automation') {
      parallel {
        stage('QA-UI-Automation') {
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
            sleep 10
            bat 'mvn test'
          }
        }

        stage('QA-API-Automation') {
          agent any
          environment {
            recipientProviders = 'emailext body: \'API and UI Automation Scripts are successful\', subject: \'All Automated Tests Are Successful\', to: \'vijayalaxmi.thilaga@gmail.com; vijayalakshmi.thilaga@gmail.com\''
          }
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppApiAutomation.git'
            sleep 10
            bat 'mvn test'
            emailext(subject: 'Pipeline - UI and API Automation', body: 'All UI and API Automation API ran successfully', attachLog: true, from: 'vijayalaxmi.jenkins@gmail.com', to: 'vijayalaxmi.thilaga@gmail.com')
          }
        }

      }
    }

  }
}
