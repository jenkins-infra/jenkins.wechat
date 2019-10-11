---
title: "引入新的文件夹授权插件"
description: "引入新的文件夹授权插件"
date: 2019-10-11
original: "https://devops.com/2019-devops-world-jenkins-world-award-winners-announced/"
tags:
- 外挂程式 安全 性能 gsoc gsoc2019
keywords:
- DEVOPS JENKINS
author:  Abhyudaya Sharma
translator: wenjunzhangp test
---

在我的 Google Summer of Code Projec t期间，我创建了全新的 Folder Auth 插件，可轻松管理 Folders 插件对文件夹中组织的项目的权限。这个新插件旨在通过易于管理的角色进行快速权限检查。该插件的1.0版本刚刚发布，可以从您的 Jenkins 更新中心下载。

该插件的灵感来自角色策略插件 ，可改善性能并简化角色管理。开发该插件是为了克服角色策略插件在许多角色上的性能限制。同时，该插件通过文件夹解决了 Jenkins 中组织项目最受欢迎的方式之一。该插件还具有一个新的UI，将来会有更多改进。
.
该插件支持三种类型的角色，分别适用于 Jenkins 中的不同位置。
* 全球角色：适用于詹金斯的所有地方
* 代理角色：限制连接到您的实例的多个代理的权限
* 文件夹角色：适用于文件夹内组织的多个作业

!(./folder-auth.png)

## 角色策略插件的性能改进
与角色策略插件不同，此插件不使用正则表达式来查找匹配的项目和代理，从而改善了我们的性能并简化了管理员的工作。为了减少需要管理的角色数量，通过文件夹角色授予文件夹的权限将继承其所有子项。这对于通过单个角色访问多个项目很有用。同样，一个代理角色可以应用于多个代理，并分配给多个用户。

此插件旨在在权限检查中胜过“角色策略插件”。使用 我在GSoC项目第一阶段中创建的微基准框架对改进进行 了评估。两个插件的相同配置的基准测试表明，与角色策略2.13中的全局角色相比，对 500 个全局角色而言，权限检查的速度最高快 934x ，后者本身包含了一些性能改进。将文件夹角色与“角色策略”的项目角色进行比较，可以对在 150 个用户的实例上组织在两层深度文件夹中的 250 个项目进行的权限检查，将访问作业的速度提高了将近 15 倍。您可以在此处查看基准和结果比较 。

## Jenkins配置作为代码支持
该插件支持 Jenkins 的“代码配置”功能，因此您无需通过 Web UI 即可配置权限。YAML配置如下所示：

``` javascript
jenkins:
  authorizationStrategy:
    folderBased:
      globalRoles:
        - name: "admin"
          permissions:
            - id: "hudson.model.Hudson.Administer"
              # ...
          sids:
            - "admin"
        - name: "read"
          permissions:
            - id: "hudson.model.Hudson.Read"
          sids:
            - "user1"
      folderRoles:
        - folders:
            - "root"
          name: "viewRoot"
          permissions:
            - id: "hudson.model.Item.Read"
          sids:
            - "user1"
      agentRoles:
        - agents:
            - "agent1"
          name: "agentRole1"
          permissions:
            - id: "hudson.model.Computer.Configure"
            - id: "hudson.model.Computer.Disconnect"
          sids:
            - "user1"
```
## 带有Swagger支持的REST API
该插件提供 REST API ，用于通过 Swagger.json 管理具有 OpenAPI 规范的角色。您可以在 SwaggerHub 上签出 Swagger API  。SwaggerHub 提供了多种语言的存根，可以下载并用于与插件进行交互。您还可以使用 curl   从命令行查看一些示例请求。

!(./swagger.png)
!(./swagger2.png)

