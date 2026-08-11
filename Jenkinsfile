pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Generate HTML Report') {
            steps {
                // 创建报告目录
                bat '''
                    if not exist my-report mkdir my-report
                '''

                // 写一个简单的 HTML 报告
                writeFile file: 'my-report/index.html', text: """
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>示例测试报告</title>
    <style>
        body {
            font-family: Arial, "Microsoft YaHei", sans-serif;
            margin: 40px;
            background: #f5f7fa;
            color: #333;
        }
        .card {
            max-width: 720px;
            background: #fff;
            border: 1px solid #e5e7eb;
            border-radius: 8px;
            padding: 24px;
        }
        h1 { color: #2563eb; margin-top: 0; }
        .ok { color: #16a34a; font-weight: bold; }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 16px;
        }
        th, td {
            border: 1px solid #e5e7eb;
            padding: 10px;
            text-align: left;
        }
        th { background: #f3f4f6; }
    </style>
</head>
<body>
    <div class="card">
        <h1>示例单元测试报告</h1>
        <p>任务：<b>${env.JOB_NAME}</b></p>
        <p>构建号：<b>#${env.BUILD_NUMBER}</b></p>
        <p>结果：<span class="ok">全部通过</span></p>

        <table>
            <tr>
                <th>用例</th>
                <th>结果</th>
                <th>耗时</th>
            </tr>
            <tr>
                <td>login_should_success</td>
                <td class="ok">PASS</td>
                <td>12 ms</td>
            </tr>
            <tr>
                <td>logout_should_clear_token</td>
                <td class="ok">PASS</td>
                <td>8 ms</td>
            </tr>
            <tr>
                <td>profile_should_load</td>
                <td class="ok">PASS</td>
                <td>25 ms</td>
            </tr>
        </table>

        <p style="margin-top: 20px; color: #6b7280;">
            这是 HTML Publisher 插件的练习报告。
        </p>
    </div>
</body>
</html>
"""
                echo 'HTML 报告已生成：my-report/index.html'
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'my-report',
                reportFiles: 'index.html',
                reportName: '示例测试报告'
            ])
        }
    }
}