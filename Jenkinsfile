pipeline {
  agent any
  stages {
    stage('Dev-Build') {
      steps {
        git 'https://github.com/Vijayalaxmi08/WebApp.git'
        script {
          try{
            retry(2){
              //Check if the server is already running, STOP IT
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
        stage('QA-UI-Automation') {
          agent{
						label{
							label ''
							customWorkspace 'workspace/WebAppUIAutomation'
						}
					}
          steps {
            git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
          }
        }

        stage('QA-API-Automation') {
          steps {
            agent{
						label{
							label ''
							customWorkspace 'workspace/WebAppAPIAutomation'
						}
					}
            git 'https://github.com/Vijayalaxmi08/WebAppApiAutomation.git'
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
          }
        }

      }
    }

  }
}
