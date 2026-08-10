pipeline {
    agent any

    stages {
        stage('构建') {
            steps {
                echo '开始构建'
                bat 'echo build-ok > build-info.txt'
            }
        }
        stage('可选：测失败邮件') {
            steps {
                echo '若要测失败通知，把下一行注释打开'
                // bat 'exit /b 1'
            }
        }
    }

    post {
        success {
            mail to: '你的QQ号@qq.com',
                 subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """结果: SUCCESS
任务: ${env.JOB_NAME}
构建: #${env.BUILD_NUMBER}
详情: ${env.BUILD_URL}
"""
        }
        failure {
            mail to: '你的QQ号@qq.com',
                 subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """结果: FAILURE
任务: ${env.JOB_NAME}
构建: #${env.BUILD_NUMBER}
详情: ${env.BUILD_URL}
请打开上面链接查看 Console Output。
"""
        }
    }
}