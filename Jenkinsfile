pipeline {
    agent any

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: '构建分支')
    }

    stages {
        stage('打印') {
            steps {
                echo "将使用分支: ${params.BRANCH}"
            }
        }
        stage('发布') {
            steps {
                bat '''
                    copy /Y index.html E:\\learn\\jenkins_learn\\test\\index.html
                    echo ${params.BRANCH} > E:\\learn\\jenkins_learn\\test\\from-branch.txt
                '''
            }
        }
    }
}