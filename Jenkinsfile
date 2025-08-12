pipeline {
    agent any
    tools{
        maven 'maven'
    }
    environment {
        SONARQUBE_URL = 'https://d6312cd2eccd.ngrok-free.app/projects'
        SONARQUBE_TOKEN = credentials ('sqa_53e9cc4c8c212b68658545e9a39394b4b8a99d36')
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', credentialsId: 'git_spring_token', url: 'https://github.com/Brahmamk3/sprintboot.git'
                sh 'ls -la'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Package'){
            steps{
                sh 'mvn package'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                sh '''
                mvn sonar:sonar \
                  -Dsonar.projectKey=springboot-demo \
                  -Dsonar.host.url=https://d6312cd2eccd.ngrok-free.app \
                  -Dsonar.login=$sonarqube_token
                '''
            }
        }
    }
}
