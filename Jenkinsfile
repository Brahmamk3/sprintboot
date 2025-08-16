pipeline {
    agent any

    triggers {
        githubPush()
    }

    tools{
        maven 'maven'
    }
    environment {

        SONARQUBE_URL = 'https://d6312cd2eccd.ngrok-free.app/projects'
        SONARQUBE_TOKEN = credentials ('')

        SONARQUBE_URL = 'http://44.202.10.13:9000/projects'
        SONARQUBE_TOKEN = credentials ('sonar-token')

    }
    stages {
        stage('Checkout'){
            steps{

                git branch: 'main', credentialsId: '', url: ''

                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/Brahmamk3/sprintboot.git'

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

                  -Dsonar.host.url=http://44.202.10.13:9000 \

                  -Dsonar.login=$SONARQUBE_TOKEN
                '''
            }
        }

    }
}

        stage('Deploy'){
            steps{
                sh '''
                nohup java -jar target/simple-veera-hello -1.0.0.jar --server.port=9000 > app.log 2>&1 &
                '''
            }
        }
    }
}

