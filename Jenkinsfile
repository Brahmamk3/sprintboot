pipeline {
    agent any

    triggers {
        githubPush()
    }

    tools {
        maven 'maven'
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
stage('Build Docker Image') {
    steps {
        sh 'docker build -t brahmamk015/simple-hello-springboot:1.0.0 .'
    }
}

stage('Push Docker Image') {
    steps {
        sh 'docker push brahmamk015/simple-hello-springboot:1.0.0'
    }
}

stage('Deploy or Run') {
    steps {
        sh 'docker run -d -p 8080:8080 --name springboot-app brahmamk015/simple-hello-springboot:1.0.0'
    }
}
 
    }
}
