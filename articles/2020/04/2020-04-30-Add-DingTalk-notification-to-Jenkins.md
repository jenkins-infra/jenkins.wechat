---
title: "在jenkins中配置钉钉通知"
description: "介绍如何在jenkins中配置钉钉通知"
author: "wuzz25861"
date: 2020-04-15
tags:
- Jenkins
- pipeline
- DingTalk
---

## 安装钉钉插件
【系统管理】-【管理插件】-【可选插件】，选择【DingTalk】插件安装
![插件安装](plugin-install.png)

## freestyle通知配置
进入项目界面-【配置】-【构建后操作】-【钉钉通知器配置】-输入jenkins地址（注意以/结尾）-填写钉钉机器人token-选择通知条件-【保存】
![插件配置](setting-1.png)
![插件配置](setting-2.png)
![插件配置](setting-3.png)

## pipeline方式
只支持Declarative Pipeline post方式，不支持Scripted Pipeline。

	post {
		success {	  
			dingTalk accessToken:'你的钉钉token', 
			imageUrl:'图片地址',
			jenkinsUrl:'你的jenkins地址/', 
			message:'提示信息',
			notifyPeople:'你想提醒的人'
		   }
		failure {
			dingTalk accessToken:'你的钉钉token', 
			imageUrl:'图片地址',
			jenkinsUrl:'你的jenkins地址/', 
			message:'提示信息',
			notifyPeople:'你想提醒的人'
			  }
		unstable {
            echo 'unstable'     
        }
        aborted {
            echo 'aborted'  
        }
        changed {
            echo 'changed'          
        } 
	}

## 构建结果通知
![结果通知](notice.png)