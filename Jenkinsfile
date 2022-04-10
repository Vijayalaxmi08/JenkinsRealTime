pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      steps {
        git 'https://github.com/Vijayalaxmi08/WebApp.git'
        retry(count: 2) {
          bat 'bat "start /min stopApp.bat"'
        }

        bat 'bat "mvn install"'
        bat 'bat "set JENKINS_NODE_COOKIE=dontKillMe && start /min startApp.bat"'
      }
    }

    stage('QA-UI-Automation') {
      parallel {
        stage('QA-UI-Automation') {
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
            sleep 10
            bat 'bat \'mvn test\''
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
            bat 'bat \'mvn test\''
            emailext(subject: 'Pipeline - UI and API Automation', body: 'All UI and API Automation API ran successfully', attachLog: true, from: 'vijayalaxmi.jenkins@gmail.com', to: 'vijayalaxmi.thilaga@gmail.com')
          }
        }

      }
    }

  }
}