pipeline {
    agent any
    tools{
        maven 'maven_3_9_8'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ManalAlkuraya/wireMock_Project']])
                sh 'mvn clean install'
            }
        }
    }
}