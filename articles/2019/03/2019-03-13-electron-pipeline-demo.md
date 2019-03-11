---
title: Electron 应用的流水线设计
description: "跨平台构建的流水线 demo"
date: 2019-03-13
tags:
    - Jenkins
    - Pipeline
    - Electron
author: zacker330
original: https://showme.codes/2019-03-10/electronjs-pipeline-demo/
---
面向读者：最好是了解 Jenkins 流水线的基本语法。

Electron 是由 Github 开发，用 HTML，CSS 和 JavaScript 来构建跨平台桌面应用程序的一个开源库。

本文将介绍 Electron 桌面应用的流水线的设计。

但是如何介绍呢？倒是个大问题。笔者尝试直接贴代码，在代码注释中讲解。这是一次尝试，希望你正负的反馈。

### 完整代码

```groovy
pipeline {
// 我们决定每一个阶段指定 agent，所以，
// 流水线的 agent 设置为 none，这样不会占用 agent
agent none
// 指定整条流水线的环境变量
environment {
  __version = ""
  __appName = "electron-webpack-quick-start"
}

stages {
  stage("生成版本号"){
    agent {label "linux" }
    steps{
      script{
          __version = generateVersion("1.0.0")
          echo "version is ${__version}"
    }}
  }
  stage('并行构建') {
    // 快速失败，只要其中一个平台构建失败，
    // 整次构建算失败
    failFast true
    // parallel 闭包内的阶段将并行执行 
    parallel {
      stage('Windows平台下构建') {
        agent {label "windows && nodejs" }
        steps {
          echo "${__version}"
        }
      }
      stage('Linux平台下构建') {
        agent {label  "linux && nodejs" }
        // 不同平台可能存在不同的环境变量
        // environment 支持阶段级的环境变量
        environment{
            __suffix = "tar.xz"
            __artifactPath = "dist/${__appName}-${__version}.${__suffix}"
        }
        steps {
          script{
            // Jenkins nodejs 插件提供的 nodejs 包装器
            // 包装器内可以执行 npm 命令。
            // nodejs10.15.2 是在 Jenkins 的全局工具配置中添加的 NodeJS 安装器
            nodejs(nodeJSInstallationName: 'nodejs10.15.2') {
              // 执行具体的构建命令
              sh "npm install yarn"
              sh "yarn version --new-version ${__version}"
              sh "yarn install"
              sh "yarn dist --linux deb ${__suffix}"
              // 上传制品
              uploadArtifact("${__appName}", "${__version}", "${__artifactPath}")
        }}} // 将括号合并是为了让代码看起来紧凑，提升阅读体验。下同。
      }
      stage('Mac平台下构建') {
        agent {label "mac && nodejs" }
        stages {
          stage('mac 下阶段1') {
            steps { echo "staging 1" }
          }
          stage('mac 下阶段2') {
            steps { echo "staging 2" }
          }
        }
  } } } 
  stage("其它阶段，读者可根据情况自行添加"){
    agent {label "linux"}
    steps{
        echo "发布"
    } } 
}
post {
  always { cleanWs() } } // 清理工作空间
}

def generateVersion(def ver){
  def gitCommitId = env.GIT_COMMIT.take(7)
  return "${ver}-${gitCommitId}.${env.BUILD_NUMBER}"
}

def uploadArtifact(def appName, def appVersion, def artifactPath){
  echo "根据参数将制品上传到制品库中，待测试"
}
```

### 代码补充说明

因为 Electron 是跨平台的，我们需要将构建过程分别放到 Windows、Linux、Mac 各平台下执行。所以，不同平台的构建任务需要执行在不同的 agent 上。我们通过在 `stage` 内定义 `agent` 实现。如在“Mac平台下构建”的阶段中，`agent {label "mac && nodejs" }` 指定了只有 label 同时包括了 mac 和 nodejs 的 agent 才能执行构建。

多平台的构建应该是并行的，以提升流水线的效率。我们通过 `parallel` 指令实现。

另外，默认 Electron 应用使用的三段式版本号设计，即 Major.Minor.Patch。但是笔者认为三段式的版本号信息还不够追踪应用与构建之间的关系。笔者希望版本号能反应出构建号和源代码的 commit id。函数 `generateVersion` 用于生成此类版本号。生成的版本号，看起来类似这样：`1.0.0-f7b06d0.28`。


### 小结
完整源码地址：https://github.com/zacker330/electronjs-pipeline-demo

### 参考：
* 持续交付的八大原则：https://blog.csdn.net/tony1130/article/details/6673741
* Jenkins nodejs 插件：https://plugins.jenkins.io/nodejs 
* Electron 版本管理：https://electronjs.org/docs/tutorial/electron-versioning#semver

