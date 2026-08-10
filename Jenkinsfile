pipeline {
    agent any
    stages {
        stage('构建') {
            steps {
                bat 'echo build'
            }
        }
        stage('确认发布') {
            steps {
                input message: '构建完成，是否发布到测试目录？', ok: '发布'
            }
        }
        stage('发布') {
            steps {
                bat 'echo deploy'
            }
        }
    }
}