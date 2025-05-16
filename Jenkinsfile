pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:8.0'
            args '-u root:root' // ถ้าต้องการสิทธิ์ root
        }
    }

  environment {
        SOLUTION = "api.sln"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/nu2499nu2/MyRo.git', branch: 'main'
            }
        }
        stage('Check .NET Version') {
            steps {
                sh 'dotnet --info'
            }
        }
        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o ./publish'
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
