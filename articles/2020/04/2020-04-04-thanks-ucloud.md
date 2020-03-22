---
title: "致 UCloud 的一份感谢信"
description: "Jenkins 中文社区会在 UCloud 的帮助下为用户提供给好的服务"
date: 2020-04-04
author: linuxsuren
poster: “./2020-04-04-thanks-ucloud/ucloud-logo.png”
---

![ucloud logo](ucloud-logo.png)

故事要从社区服务器的几起故障说起。某天，在微信群、公众号后台等几个渠道中，有用户反馈说 Jenkins 中文社区提供的插件更新中心国内镜像源无法访问。为了避免影响到更多的人，我们赶紧在社区的管理群中同步了该消息。在没有专业运维人员帮助的情况下，我们也必须量快地排除服务器故障。从服务器资源使用的图表上，我们看到 CPU、网络都已经跑满了，此时，已经是无法远程登录到服务器做任何操作。对于社区的现状而言，确实有一些窘迫，只有1M带宽、1核CPU、1G内存。先不去考虑并发量的问题，单个用户的请求无法得到较好的体验。这对于完全是由小伙伴自发、志愿组成的开源、公益社区，唯一的出路就是寻找外部的资源和支持，尤其是对开源愿意扶持、有社会责任感的企业。

此时，我想要分享给社区小伙伴的好消息是：在去年11月份收到了来自[霍格沃兹测试学院](https://jenkins-zh.cn/wechat/articles/2019/11/2019-11-13-a-thanks-letter/)给予的无私赞助外，今年3月份再次收到了**长期专注于移动互联网领域**的基础云计算服务提供商 [UCloud](https://www.ucloud.cn/) 的大力支持。UCloud 将会为 Jenkins 中文社区提供1年免费的服务器的使用，社区的基础设施也会因此得到增强。我们相信，Jenkins 中文社区将会在 UCloud 的帮助下，可以继续带来更多的社会价值，服务更多的个人以及团体用户。

自打发布了[Jenkins 插件中心国内镜像源](https://jenkins-zh.cn/wechat/articles/2019/11/2019-11-11-update-center-mirror-announcement/)后，我们收到了很多的来自[社区论坛](https://community.jenkins-zh.cn/t/jenkins/26)以及 [GitHub Issues](https://github.com/jenkinsci/localization-zh-cn-plugin/issues?q=is%3Aissue+is%3Aopen+update+center) 上的反馈。根据反馈社区也做了很多的改进措施，包括：[Jenkins CLI](https://github.com/jenkins-zh/jenkins-cli) 对切换镜像源的支持、无需任何配置即可使用 Jenkins 中国镜像源的[开箱即用](https://github.com/jenkins-zh/docker-zh)的方案。当然，除了软件方案上的改进措施外，我们今天想要特别提到的就是社区基础设施的增强——我们会把 Jenkins 国内镜像源相关的服务全部迁移到 UCloud 的云服务器上。如果您对国内源的访问情况感兴趣，可以查看这里的[统计图表](https://jenkins-zh.github.io/update-center-mirror/data)。

公开透明，对于开源社区是非常重要的，我们会把所有收到的赞助以及资源的使用情况，全部记录到 [Jenkins 中文社区官网](https://jenkins-zh.cn/about/sponsors-list/)，同时也欢迎每一位关注社区的人来监督。
