pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'gradle test'
            }
        }
    }
    post {
        always {
            publishHTML(
                target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'api/build/reports/test',
                    reportFiles: 'index.html',
                    reportName: 'API 测试报告'
                ]
            )
        }
    }
}