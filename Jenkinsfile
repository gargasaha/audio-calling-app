pipeline {
    agent any

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
        BUILD_CONFIGURATION = 'Release'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
                // For Windows agent, use:
                // bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh "dotnet build --configuration ${BUILD_CONFIGURATION} --no-restore"
                // Windows:
                // bat "dotnet build --configuration %BUILD_CONFIGURATION% --no-restore"
            }
        }

        stage('Run Tests') {
            steps {
                sh "dotnet test --configuration ${BUILD_CONFIGURATION} --no-build"
                // Windows:
                // bat "dotnet test --configuration %BUILD_CONFIGURATION% --no-build"
            }
        }

        stage('Publish') {
            steps {
                sh """
                dotnet publish \
                --configuration ${BUILD_CONFIGURATION} \
                --no-build \
                --output publish
                """
                // Windows:
                // bat 'dotnet publish --configuration %BUILD_CONFIGURATION% --no-build --output publish'
            }
        }
        stage('Publish') {
        steps {
            bat """
            dotnet publish ^
            --configuration Release ^
            --no-build ^
            --output "C:\\Users\\sahag\\OneDrive\\Desktop\\c#\\published"
            """
    }
}

    }

    post {
        success {
            echo '✅ Build and publish successful'
        }
        failure {
            echo '❌ Build failed'
        }
        always {
            archiveArtifacts artifacts: 'publish/**', fingerprint: true
        }
    }
}
