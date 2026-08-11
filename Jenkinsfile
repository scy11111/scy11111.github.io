pipeline {
    agent any
    options { skipDefaultCheckout() }
    stages {
        stage('Use credentials') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-https',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_PASS'
                    )
                ]) {
                    bat 'echo 已绑定用户 %GIT_USER% （请勿打印密码）'
                }
            }
        }
    }
}