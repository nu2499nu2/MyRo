pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:8.0'
            args '-u root:root' // ถ้าต้องการสิทธิ์ root
        }
    }

    environment {
        DOTNET_ROOT = "${HOME}/.dotnet"
        PATH = "${DOTNET_ROOT}:${PATH}"
        DOTNET_CLI_TELEMETRY_OPTOUT = "1"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/nu2499nu2/MyRo.git', branch: 'main'
            }
        }
        stage('Check .NET Version') {
            steps {
                sh '${DOTNET_ROOT}/dotnet --info'
            }
        }
        stage('Restore') {
            steps {
                sh '${DOTNET_ROOT}/dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh '${DOTNET_ROOT}/dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh '${DOTNET_ROOT}/dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh '${DOTNET_ROOT}/dotnet publish -c Release -o ./publish'
            }
        }

        stage('Deploy') {
            steps {
                // Replace this with your actual deploy method (Docker, SCP to server, etc.)
                echo 'Deploying the application...'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/publish/**', fingerprint: true
        }
    }
}
