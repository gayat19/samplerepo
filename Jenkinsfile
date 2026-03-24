pipeline {
    agent any

    stages {
        stage('Build and Test in Docker') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:8.0'
                    args '-u root:root'
                }
            }
            steps {
                checkout scm
                sh 'dotnet restore'
                sh 'dotnet build --no-restore'
                sh 'dotnet test --no-build --logger "trx;LogFileName=test-results.trx"'
                sh 'dotnet publish DemoApp/DemoApp.csproj -c Release -o publish'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'publish/**', allowEmptyArchive: true
                    archiveArtifacts artifacts: '**/*.trx', allowEmptyArchive: true
                }
            }
        }
    }
}