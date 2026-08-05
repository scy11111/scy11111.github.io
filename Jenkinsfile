pipeline {
    agent any

    parameters {
        string(name: 'APP_VERSION', defaultValue: 'local', description: '版本号，如 v1.0.0')
    }

    stages {
        stage('备份当前测试环境') {
            steps {
                bat '''
                    if exist E:\\learn\\jenkins_learn\\test\\index.html (
                      copy /Y E:\\learn\\jenkins_learn\\test\\index.html E:\\learn\\jenkins_learn\\backup\\index.html.prev
                    )
                '''
            }
        }
        stage('发布') {
            steps {
                bat """
                    copy /Y index.html E:\\learn\\jenkins_learn\\test\\index.html
                    echo ${params.APP_VERSION} > E:\\learn\\jenkins_learn\\test\\version.txt
                """
            }
        }
    }
}