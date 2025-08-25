pipeline {
  agent any

  environment {
    // Avoid interactive host key prompts during Ansible SSH
    ANSIBLE_HOST_KEY_CHECKING = 'False'
    APP_SERVER_IP = "18.170.1.231"   // replace with your app server IP
  }

  stages {

    stage('Checkout') {
      steps {
        // Checkout Spring Boot repo
        git(
          url: 'https://github.com/Brahmamk3/sprintboot.git',
          branch: 'main'
        )
      }
    }

    stage('Build with Maven') {
      steps {
        sh 'mvn clean package -DskipTests'
        // rename jar for predictability
        sh 'cp target/*.jar target/app.jar'
      }
    }

    stage('Prepare Deploy Files') {
      steps {
        // write inventory dynamically
        writeFile file: 'inventory.ini', text: "[appserver]\n${APP_SERVER_IP} ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/veera-brahmam.pem\n"

        // write ansible playbook dynamically
        writeFile file: 'deploy.yml', text: '''
- hosts: appserver
  become: yes
  tasks:
    - name: Ensure Java 17 is present
      apt:
        name: openjdk-17-jdk
        state: present
        update_cache: yes

    - name: Create app directory
      file:
        path: /opt/app
        state: directory
        mode: '0755'

    - name: Copy built JAR to server
      copy:
        src: target/app.jar
        dest: /opt/app/app.jar
        mode: '0755'

    - name: Create systemd service for Spring Boot app
      copy:
        dest: /etc/systemd/system/myapp.service
        content: |
          [Unit]
          Description=Spring Boot App
          After=network.target

          [Service]
          User=ubuntu
          WorkingDirectory=/opt/app
          ExecStart=/usr/bin/java -jar /opt/app/app.jar
          Restart=always
          RestartSec=5

          [Install]
          WantedBy=multi-user.target

    - name: Reload systemd daemon
      systemd:
        daemon_reload: yes

    - name: Restart and enable myapp
      systemd:
        name: myapp
        state: restarted
        enabled: yes
'''
      }
    }

    stage('Deploy to App Server (Ansible)') {
      steps {
        // use Jenkins SSH credentials (replace with your ID)
        sshagent (credentials: ['ram-ssh']) {
          sh 'ansible-playbook -i inventory.ini deploy.yml'
        }
      }
    }
  }

  post {
    success {
      echo "✅ Deployment complete. Visit http://${APP_SERVER_IP}:8080"
    }
    failure {
      echo "❌ Deployment failed. Check console log for details."
    }
  }
}
