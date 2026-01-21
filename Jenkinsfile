pipeline {
    agent any

    environment {
        BUILD_CONFIGURATION = 'Release'
        PUBLISH_PATH = 'C:\\Users\\sahag\\OneDrive\\Desktop\\c#\\published'
        IIS_SITE = 'AudioCallingSite'
        IIS_PATH = 'C:\\inetpub\\AudioCallingApp'
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --configuration %BUILD_CONFIGURATION% --no-restore'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'dotnet test --configuration %BUILD_CONFIGURATION% --no-build'
            }
        }

        stage('Publish') {
            steps {
                bat """
                dotnet publish ^
                --configuration %BUILD_CONFIGURATION% ^
                --no-build ^
                --output "%PUBLISH_PATH%"
                """
            }
        }

        stage('Stop IIS Site') {
            steps {
                bat """
                %windir%\\system32\\inetsrv\\appcmd stop site "%IIS_SITE%"
                """
            }
        }

        stage('Deploy to IIS') {
            steps {
                bat """
                xcopy /Y /E "%PUBLISH_PATH%\\*" "%IIS_PATH%\\"
                """
            }
        }

        stage('Start IIS Site') {
            steps {
                bat """
                %windir%\\system32\\inetsrv\\appcmd start site "%IIS_SITE%"
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build, Publish & IIS Deployment successful'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
