@Library('my-lib') _

pipeline {
    agent any
    stages {
        stage('调用共享库') {
            steps {
                script {
                    sayHello('scy')
                }
            }
        }
    }
}