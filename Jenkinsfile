pipeline {
    agent any
    stages {
        stage('Dev-Build') {
            steps {
                // Pull the code from a GitHub repository
                git 'https://github.com/Vijayalaxmi08/WebApp.git'
				script{
					try{
						retry(2){
							//Check if the server is already running, STOP IT
							bat "start /min stopApp.bat"
						}	
					}catch(Exception e){
						echo 'There is no app running in port 9002'
					}
				}
				//maven build for QA
				bat "mvn install"
				
				//Start the server
				bat "set JENKINS_NODE_COOKIE=dontKillMe && start /min startApp.bat"	
            }
        }
		stage('Test-Automation'){
			parallel{
				
				stage('QA-UI-Automation'){
					agent{
						label{
							label ''
							customWorkspace 'workspace/WebAppUiAutomation'
						}
					}
		
					steps{
						//Pull Latest code from github repo
						git 'https://github.com/Vijayalaxmi08/WebAppUiAutomation.git'
						
						//Wait for 10 seconds
						sleep(time:10, unit:'SECONDS')
						
						//Start the selenium tests
						bat 'mvn test'
					}
				}
		
				stage('QA-API-Automation'){
					agent{
						label{
							label ''
							customWorkspace 'workspace/WebAppAPIAutomation'
						}
					}
				
					steps{
						//Pull Latest code from github repo
						git 'https://github.com/Vijayalaxmi08/WebAppApiAutomation.git'
						
						//Wait for 10 seconds
						sleep(time:10, unit:'SECONDS')
						
						//Start the selenium tests
						bat 'mvn test'
					}
				}
				
			}
			post{
				success{
					emailext body: 'API and UI Automation Scripts are successful', subject: 'All Automated Tests Are Successful', to: 'vijayalaxmi.thilaga@gmail.com; vijayalakshmi.thilaga@gmail.com'
				}
			}
		}
		
    }		
}
