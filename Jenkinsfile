pipeline {
    agent any

    environment {
        BUILD_CONFIGURATION = 'Release'
        TEMP_PUBLISH_PATH = 'C:\\deploy\\publish'
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

        stage('Restore') {
            steps {
                bat 'dotnet restore AudioCalling.csproj'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build AudioCalling.csproj -c %BUILD_CONFIGURATION% --no-restore'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test AudioCalling.csproj -c %BUILD_CONFIGURATION% --no-build'
            }
        }

        stage('Publish to Temp Folder') {
            steps {
                bat """
                if exist "%TEMP_PUBLISH_PATH%" rmdir /s /q "%TEMP_PUBLISH_PATH%"
                mkdir "%TEMP_PUBLISH_PATH%"

                dotnet publish AudioCalling.csproj ^
                -c %BUILD_CONFIGURATION% ^
                --no-build ^
                -o "%TEMP_PUBLISH_PATH%"
                """
            }
        }

        stage('Stop IIS Site') {
            steps {
                bat '%windir%\\system32\\inetsrv\\appcmd stop site "%IIS_SITE%"'
            }
        }

        stage('Deploy to IIS') {
            steps {
                bat """
                xcopy /E /Y "%TEMP_PUBLISH_PATH%\\*" "%IIS_PATH%\\"
                """
            }
        }

        stage('Start IIS Site') {
            steps {
                bat '%windir%\\system32\\inetsrv\\appcmd start site "%IIS_SITE%"'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD completed successfully'
        }
        failure {
            echo '❌ CI/CD failed'
        }
    }
}
