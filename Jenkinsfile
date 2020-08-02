properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any

    stages {
        stage('hello') {
            steps {
                // Get some code from a GitHub repository
                echo 'hello'
            }
        }
        stage('clone') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'master',
                url: 'https://github.com/pwujczyk/ProductivityTools.HttpEcho'
            }
        }
		  stage('build') {
            steps {
                bat(script: "dotnet publish ProductivityTools.HttpEcho.sln -c Release ", returnStdout: true)
            }
        }
		
		stage('stopEchoOnIis') {
            steps {
                bat('%windir%\\system32\\inetsrv\\appcmd stop site /site.name:Echo')
            }
        }

        stage('deleteIisDir') {
            steps {
                retry(5) {
                    bat('if exist "C:\\Bin\\Echo" RMDIR /Q/S "C:\\Bin\\Echo"')
                }

            }
        }
        stage('copyIisFiles') {
            steps {
                bat('xcopy "C:\\Program Files (x86)\\Jenkins\\workspace\\Meetings\\src\\Server\\ProductivityTools.Echo.WebApi\\bin\\Release\\netcoreapp3.1\\publish" "C:\\Bin\\Echo\\" /O /X /E /H /K')
				                      
            }
        }

        stage('startMeetingsOnIis') {
            steps {
                bat('%windir%\\system32\\inetsrv\\appcmd start site /site.name:Echo')
            }
        }
		
		
        stage('byebye') {
            steps {
                // Get some code from a GitHub repository
                echo 'byebye'
            }
        }
    }
}