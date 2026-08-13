pipeline {
    agent any

    parameters {
        string(
            name: 'USER_NAME',
            defaultValue: 'scy',
            description: '你的名字'
        )
        choice(
            name: 'ENV',
            choices: ['dev', 'test', 'prod'],
            description: '要部署到哪个环境'
        )
        booleanParam(
            name: 'RUN_TEST',
            defaultValue: true,
            description: '是否执行测试阶段'
        )
password(
    name: 'DEMO_PASSWORD',
    defaultValue: '',
    description: '演示用密码（输入会隐藏）'
)
    }

    stages {
        stage('打印参数') {
            steps {
                echo "用户名: ${params.USER_NAME}"
                echo "环境: ${params.ENV}"
                echo "是否跑测试: ${params.RUN_TEST}"
            }
        }

        stage('测试') {
            when {
                expression { return params.RUN_TEST }
            }
            steps {
                echo "正在 ${params.ENV} 环境执行测试..."
            }
        }

        stage('跳过提示') {
            when {
                expression { return !params.RUN_TEST }
            }
            steps {
                echo "已勾选不跑测试，跳过测试阶段"
            }
        }
    }
}