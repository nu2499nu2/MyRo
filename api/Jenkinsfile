pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${HOME}/.dotnet"
        PATH = "${DOTNET_ROOT}:${PATH}"
    }

    tools {
        // ��ͧ�Դ��� dotnet cli �� Global Tool ������ҡ script
        // ���������� tools �����ҡ�Դ�����������ͧ����
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/SoftWareNu/devop01.git', branch: 'main'
            }
        }

        stage('Restore') {
            steps {
                sh '${HOME}/.dotnet/dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh '${HOME}/.dotnet/dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh '${HOME}/.dotnet/dotnet test --no-restore --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh '${HOME}/.dotnet/dotnet publish -c Release -o out'
            }
        }

        stage('Deploy') {
            steps {
                // �س����ö deploy ���� SCP, Docker, ���� rsync
                // ������ҧ rsync:
                sh 'rsync -avz out/ user@server:/var/www/api/'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/out/**', fingerprint: true
        }
        failure {
            mail to: 'nunldevelopment@gmail.com',
                 subject: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "Something is wrong. Check Jenkins: ${env.BUILD_URL}"
        }
    }
}
