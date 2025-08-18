pipeline {
    agent any

    triggers {
        githubPush()
    }

    tools {
        maven 'maven'
    }

    environment {
        SONARQUBE_URL = 'http://13.42.48.187:9000/'
        SONARQUBE_TOKEN = credentials('sonarqube')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Brahmamk3/sprintboot.git'
                sh 'ls -la'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh """
                mvn sonar:sonar \
                  -Dsonar.projectKey=springboot-demo \
                  -Dsonar.host.url=$SONARQUBE_URL \
                  -Dsonar.login=$SONARQUBE_TOKEN
                """
            }
        }

        stage('Deploy') {
            steps {
                sh """
                nohup java -jar target/simple-veera-hello-1.0.0.jar --server.port=9000 > app.log 2>&1 &
                """
            }
        }
    }
}
