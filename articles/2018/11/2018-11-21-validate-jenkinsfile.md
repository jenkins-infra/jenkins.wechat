---
title: 在 VS Code 中校验您的 Jenkinsfile
description: 在 VS Code 中校验您的 Jenkinsfile
author: janjoerke
translator: 赵晓杰
---

在我的日常工作中，经常需要创建或修改很多 Jenkinsfile，而且还会发生错误。这是一个非常繁琐的流程，修改 Jenkinsfile，提交、推送，然后等您的 Jenkins 服务器说你少加了一个括号。

流水线命令行 Linter(https://jenkins.io/doc/book/pipeline/development/) 可以减少编写 Jenkinsfile 的调试时间，但使用起来不方便。您需要像 curl 或 ssh 的工具来连接您的 Jenkins 服务器，还需要正确地记住命令来验证您的 Jenkinsfile。我对这个方案还是不够满意。

鉴于每天都会使用 VS Code，于是我开始着手为此研发插件，使得校验 Jenkinsfile 变得更加方便。

'Jenkins Pipeline Linter Connector' 做的就是把当前打开的文件推送到您的 Jenkins 服务器，然后在 VS Code 中显示校验结果。

[Jenkins Pipeline Linter Connector | 示例 1](/images/vscode-pipeline-linter/example1.gif)

[Jenkins Pipeline Linter Connector | 示例 2](/images/vscode-pipeline-linter/example2.gif)

您可以在 VS Code 插件浏览器中或听过下面的地址 https://marketplace.visualstudio.com/items?itemName=janjoerke.jenkins-pipeline-linter-connector 找到插件。

该插件会在 VS Code 中添加四项配置，您需要配置用来执行检验的 Jenkins 服务器。

* `jenkins.pipeline.linter.connector.url` 是您的 Jenkins 服务器期待的 POST 请求地址，包含您要校验的 Jenkinsfile 文件。通常为  *http://<your_jenkins_server:port>/pipeline-model-converter/validate*。
* `jenkins.pipeline.linter.connector.user` 允许您指定您的 Jenkins 用户名。
* `jenkins.pipeline.linter.connector.pass` 允许您指定您的 Jenkins 密码。
* `jenkins.pipeline.linter.connector.crumbUrl` 当您的 Jenkins 服务器启用了 CRSF 时必须指定。通常为 *http://<your_jenkins_server:port>/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,%22:%22,//crumb)*。
​