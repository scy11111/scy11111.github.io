pipeline {
    agent any

    stages {
        stage('准备') {
            steps {
                echo "开始发布练习"
                bat 'if not exist E:\\learn\\jenkins_learn\\test mkdir E:\\learn\\jenkins_learn\\test'
            }
        }
        stage('构建(模拟)') {
            steps {
                // 静态站可省略编译；有 index.html 就行
                bat 'echo build-ok > build-info.txt'
            }
        }
        stage('发布到测试目录') {
            steps {
                bat '''
                    copy /Y index.html E:\\learn\\jenkins_learn\\test\\index.html
                    copy /Y build-info.txt E:\\learn\\jenkins_learn\\test\\build-info.txt
                    echo %DATE% %TIME% > E:\\learn\\jenkins_learn\\test\\deployed-at.txt
                '''
            }
        }
    }

    post {
        success { echo '已发布到 E:\\learn\\jenkins_learn\\test' }
        failure { echo '发布失败，看 Console' }
    }
}