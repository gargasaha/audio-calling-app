pipeline {
    agent any

    environment {
        BUILD_CONFIGURATION = 'Release'
        PUBLISH_PATH = 'C:\\Users\\sahag\\OneDrive\\Desktop\\c#\\published'
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
    }

    post {
        success {
            echo '✅ Build & Publish successful'
        }
        failure {
            echo '❌ Build failed'
        }
    }
}
