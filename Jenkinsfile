pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:8.0'
            args '-u root:root'
        }
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
                sh 'dotnet test --no-build --logger "trx;LogFileName=test-results.trx"'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish DemoApp/DemoApp.csproj -c Release -o publish'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'publish/**', fingerprint: true
            archiveArtifacts artifacts: '**/*.trx', fingerprint: true, allowEmptyArchive: true
        }
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}