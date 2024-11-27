pipeline {
    agent any
    tools {
        git 'Default'
    }
    stages {
        stage('GetProject') {
            steps {
                git branch: 'main', url: 'https://github.com/cdtm0124/test.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean:clean'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts allowEmptyArchive: true, artifacts: '**/target/*.war'
            }
        }
        stage('Deploy') {
            steps {
                sh 'sudo docker build -f Dockerfile -t myapp .'
                sh 'sudo docker rm -f "myappcontainer" || true'
                sh 'sudo docker run --name "myappcontainer" -p 8081:8080 --detach myapp:latest'
            }
        }
    }
}