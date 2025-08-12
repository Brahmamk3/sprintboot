pipeline {
    agent any
    tools{
        maven 'maven'
    }
    environment {
        SONARQUBE_URL = 'https://d6312cd2eccd.ngrok-free.app/projects'
        SONARQUBE_TOKEN = credentials ('sonarqube_token')
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
                  -Dsonar.login=$SONARQUBE_TOKEN
                '''
            }
        }
    }
}
