pipeline {
    agent any

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        DOTNET_CLI_TELEMETRY_OPTOUT = "1"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/nu2499nu2/MyRo.git', branch: 'main'
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
        failure {
            mail to: 'you@example.com',
                 subject: "Build failed in Jenkins: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Check Jenkins for details: ${env.BUILD_URL}"
        }
    }
}
