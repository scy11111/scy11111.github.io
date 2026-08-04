pipeline {
    agent any

    stages {
        stage('准备') {
            steps {
                echo '开始构建'
            }
        }
        stage('执行') {
            steps {
                bat 'echo hello-from-jenkinsfile > build-result.txt'
                bat 'type build-result.txt'
            }
        }
    }

    post {
        success { echo '构建成功' }
        failure { echo '构建失败，去看 Console Output' }
    }
}