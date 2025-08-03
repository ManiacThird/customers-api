pipeline {
    agent any
    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:$PATH"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ManiacThird/customers-api.git'
            }
        }
        stage('Check .NET Version') {
            steps {
                sh 'dotnet --version'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t customers-api .'
            }
        }
        stage('Run Docker') {
            steps {
                sh 'docker run -d -p 5002:80 customers-api'
            }
        }
    }
}
