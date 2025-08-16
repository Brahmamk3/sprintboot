pipeline {
    agent any
<<<<<<< HEAD
=======
    triggers {
        githubPush()
    }
>>>>>>> deed90967edcb035123c25a8ea49e016b5e3fd66
    tools{
        maven 'maven'
    }
    environment {
<<<<<<< HEAD
        SONARQUBE_URL = 'https://d6312cd2eccd.ngrok-free.app/projects'
        SONARQUBE_TOKEN = credentials ('')
=======
        SONARQUBE_URL = 'http://44.202.10.13:9000/projects'
        SONARQUBE_TOKEN = credentials ('sonar-token')
>>>>>>> deed90967edcb035123c25a8ea49e016b5e3fd66
    }
    stages {
        stage('Checkout'){
            steps{
<<<<<<< HEAD
                git branch: 'main', credentialsId: '', url: ''
=======
                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/Brahmamk3/sprintboot.git'
>>>>>>> deed90967edcb035123c25a8ea49e016b5e3fd66
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
<<<<<<< HEAD
                  -Dsonar.host.url=https://d6312cd2eccd.ngrok-free.app \
=======
                  -Dsonar.host.url=http://44.202.10.13:9000 \
>>>>>>> deed90967edcb035123c25a8ea49e016b5e3fd66
                  -Dsonar.login=$SONARQUBE_TOKEN
                '''
            }
        }
<<<<<<< HEAD
    }
}
=======
        stage('Deploy'){
            steps{
                sh '''
                nohup java -jar target/simple-veera-hello -1.0.0.jar --server.port=9000 > app.log 2>&1 &
                '''
            }
        }
    }
}
>>>>>>> deed90967edcb035123c25a8ea49e016b5e3fd66
