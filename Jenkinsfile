pipeline {
    agent any
    stages {
        stage('Info') {
            steps {
                echo "BRANCH_NAME=${env.BRANCH_NAME}"
                bat 'git rev-parse --abbrev-ref HEAD'
            }
        }
        stage('Build') {
            steps {
                bat 'echo multibranch-ok > build-info.txt'
            }
        }
    }
}