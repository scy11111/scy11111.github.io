pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'FORCE_FAILURE',
            defaultValue: false,
            description: '勾选后故意让构建失败，用于测试失败邮件'
        )
    }

    options {
        timestamps()
		skipDefaultCheckout()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '开始执行构建……'

                // 这里换成你实际的 Android 构建命令
                // bat 'gradlew.bat clean assembleDebug'

                echo '构建完成'
            }
        }

        stage('Generate Report') {
            steps {
                // 生成一个示例附件
                writeFile(
                    file: 'build-report.txt',
                    text: """\
Jenkins 构建报告
================

任务名称：${env.JOB_NAME}
构建编号：${env.BUILD_NUMBER}
工作目录：${env.WORKSPACE}
构建地址：${env.BUILD_URL}

这只是一个 email-ext 附件示例。
"""
                )
            }
        }

        stage('Test Failure Mail') {
            when {
                expression {
                    return params.FORCE_FAILURE
                }
            }

            steps {
                error('这是为了测试失败邮件而主动制造的错误')
            }
        }
    }

    post {
        always {
            script {
                /*
                 * Jenkins 在流水线执行期间，currentBuild.result 可能为 null，
                 * 因此优先使用 currentResult。
                 */
                def buildResult = currentBuild.currentResult

                def statusColor
                def statusText
                def statusIcon

                switch (buildResult) {
                    case 'SUCCESS':
                        statusColor = '#28a745'
                        statusText = '构建成功'
                        statusIcon = '✅'
                        break

                    case 'FAILURE':
                        statusColor = '#dc3545'
                        statusText = '构建失败'
                        statusIcon = '❌'
                        break

                    case 'UNSTABLE':
                        statusColor = '#ffc107'
                        statusText = '构建不稳定'
                        statusIcon = '⚠️'
                        break

                    case 'ABORTED':
                        statusColor = '#6c757d'
                        statusText = '构建已取消'
                        statusIcon = '⏹️'
                        break

                    default:
                        statusColor = '#17a2b8'
                        statusText = buildResult
                        statusIcon = 'ℹ️'
                }

                def branchName = env.BRANCH_NAME ?: env.GIT_BRANCH ?: '未知'
                def commitId = env.GIT_COMMIT ?: '未知'
                def buildDuration = currentBuild.durationString
                        ?.replace(' and counting', '') ?: '未知'

                def emailSubject =
                    "${statusIcon} ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${statusText}"

                def emailBody = """
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <style>
        body {
            margin: 0;
            padding: 20px;
            background-color: #f5f7fa;
            font-family: Arial, "Microsoft YaHei", sans-serif;
            color: #333333;
        }

        .container {
            max-width: 760px;
            margin: 0 auto;
            background-color: #ffffff;
            border: 1px solid #e5e7eb;
            border-radius: 8px;
            overflow: hidden;
        }

        .header {
            padding: 24px;
            color: #ffffff;
            background-color: ${statusColor};
        }

        .header h1 {
            margin: 0 0 8px;
            font-size: 24px;
        }

        .header p {
            margin: 0;
            opacity: 0.9;
        }

        .content {
            padding: 24px;
        }

        .result {
            margin-bottom: 22px;
            padding: 16px;
            border-left: 5px solid ${statusColor};
            background-color: #f8f9fa;
        }

        .result strong {
            color: ${statusColor};
            font-size: 18px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 12px;
        }

        th, td {
            padding: 11px 12px;
            border: 1px solid #e5e7eb;
            text-align: left;
            word-break: break-all;
        }

        th {
            width: 150px;
            background-color: #f3f4f6;
        }

        .button {
            display: inline-block;
            margin-top: 22px;
            padding: 11px 20px;
            color: #ffffff !important;
            background-color: ${statusColor};
            border-radius: 5px;
            text-decoration: none;
        }

        .notice {
            margin-top: 22px;
            padding: 14px;
            color: #664d03;
            background-color: #fff3cd;
            border: 1px solid #ffecb5;
            border-radius: 5px;
        }

        .footer {
            padding: 18px 24px;
            background-color: #f3f4f6;
            color: #6b7280;
            font-size: 12px;
            line-height: 1.7;
        }
    </style>
</head>

<body>
<div class="container">
    <div class="header">
        <h1>${statusIcon} ${statusText}</h1>
        <p>Jenkins 自动构建通知</p>
    </div>

    <div class="content">
        <p>你好：</p>

        <p>
            Jenkins 任务
            <strong>${env.JOB_NAME}</strong>
            的第 <strong>${env.BUILD_NUMBER}</strong> 次构建已经执行完毕。
        </p>

        <div class="result">
            本次构建结果：
            <strong>${statusText}</strong>
        </div>

        <h3>构建信息</h3>

        <table>
            <tr>
                <th>任务名称</th>
                <td>${env.JOB_NAME}</td>
            </tr>
            <tr>
                <th>构建编号</th>
                <td>#${env.BUILD_NUMBER}</td>
            </tr>
            <tr>
                <th>构建结果</th>
                <td style="color: ${statusColor}; font-weight: bold;">
                    ${buildResult}
                </td>
            </tr>
            <tr>
                <th>Git 分支</th>
                <td>${branchName}</td>
            </tr>
            <tr>
                <th>Git Commit</th>
                <td>${commitId}</td>
            </tr>
            <tr>
                <th>构建耗时</th>
                <td>${buildDuration}</td>
            </tr>
            <tr>
                <th>工作目录</th>
                <td>${env.WORKSPACE}</td>
            </tr>
            <tr>
                <th>Jenkins 节点</th>
                <td>${env.NODE_NAME}</td>
            </tr>
        </table>

        <a class="button" href="${env.BUILD_URL}">
            查看 Jenkins 构建详情
        </a>

        <div class="notice">
            <strong>附件说明：</strong>
            本邮件附带了构建报告和压缩后的 Jenkins 控制台日志。
            如果构建失败，可以优先查看日志末尾的错误信息。
        </div>

        <p style="margin-top: 24px;">
            如果你不是本任务的相关人员，或者不需要接收此类通知，
            请联系 Jenkins 管理员调整收件人设置。
        </p>
    </div>

    <div class="footer">
        此邮件由 Jenkins 自动发送，请勿直接回复。<br>
        Reply-To 地址可以用于联系构建维护人员。<br>
        任务地址：${env.JOB_URL}
    </div>
</div>
</body>
</html>
"""

                emailext(
                    // 普通固定收件人
                    to: '312689079@qq.com',

                    // 回复该邮件时使用的地址
                    replyTo: '312689079@qq.com',

                    subject: emailSubject,
                    body: emailBody,

                    // 指定正文为 HTML
                    mimeType: 'text/html',

                    // 附加前面生成的报告
                    attachmentsPattern: 'build-report.txt',

                    // 附加 Jenkins 控制台日志
                    attachLog: true,

                    // 压缩控制台日志
                    compressLog: true,

                    /*
                     * 自动添加角色收件人。
                     *
                     * Developers：本次构建涉及提交的开发者
                     * Requester：手动触发构建的人
                     * Culprits：导致构建持续失败的相关提交者
                     *
                     * 如果 Git 提交邮箱没有对应可投递地址，
                     * Developers 或 Culprits 可能收不到邮件。
                     */
                    recipientProviders: [
                        [$class: 'DevelopersRecipientProvider'],
                        [$class: 'RequesterRecipientProvider'],
                        [$class: 'CulpritsRecipientProvider']
                    ]

                    /*
                     * 可选：预发送脚本。
                     * 首次使用可能需要管理员进行脚本审批。
                     * 建议先不要启用，确认基础邮件发送成功后再试。
                     *
                     * presendScript: '''
                     *     logger.println("准备发送扩展邮件")
                     *
                     *     // 返回 true 时取消发送
                     *     cancel = false
                     * ''',
                     *
                     * 可选：发送后脚本。
                     *
                     * postsendScript: '''
                     *     logger.println("扩展邮件发送处理结束")
                     * '''
                     */
                )
            }
        }
    }
}