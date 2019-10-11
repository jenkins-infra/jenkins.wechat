---
title: "�����µ��ļ�����Ȩ���"
description: "�����µ��ļ�����Ȩ���"
date: 2019-10-11
original: "https://devops.com/2019-devops-world-jenkins-world-award-winners-announced/"
tags:
- ��ҳ�ʽ ��ȫ ���� gsoc gsoc2019
keywords:
- DEVOPS JENKINS
author:  Abhyudaya Sharma
translator: wenjunzhangp test
---

���ҵ� Google Summer of Code Projec t�ڼ䣬�Ҵ�����ȫ�µ� Folder Auth ����������ɹ��� Folders ������ļ�������֯����Ŀ��Ȩ�ޡ�����²��ּ��ͨ�����ڹ���Ľ�ɫ���п���Ȩ�޼�顣�ò����1.0�汾�ոշ��������Դ����� Jenkins �����������ء�

�ò����������Խ�ɫ���Բ�� ���ɸ������ܲ��򻯽�ɫ���������ò����Ϊ�˿˷���ɫ���Բ��������ɫ�ϵ��������ơ�ͬʱ���ò��ͨ���ļ��н���� Jenkins ����֯��Ŀ���ܻ�ӭ�ķ�ʽ֮һ���ò��������һ���µ�UI���������и���Ľ���
.
�ò��֧���������͵Ľ�ɫ���ֱ������� Jenkins �еĲ�ͬλ�á�
* ȫ���ɫ��������ղ��˹�����еط�
* �����ɫ���������ӵ�����ʵ���Ķ�������Ȩ��
* �ļ��н�ɫ���������ļ�������֯�Ķ����ҵ

!(./folder-auth.png)

## ��ɫ���Բ�������ܸĽ�
���ɫ���Բ����ͬ���˲����ʹ��������ʽ������ƥ�����Ŀ�ʹ����Ӷ����������ǵ����ܲ����˹���Ա�Ĺ�����Ϊ�˼�����Ҫ����Ľ�ɫ������ͨ���ļ��н�ɫ�����ļ��е�Ȩ�޽��̳���������������ͨ��������ɫ���ʶ����Ŀ�����á�ͬ����һ�������ɫ����Ӧ���ڶ�����������������û���

�˲��ּ����Ȩ�޼����ʤ������ɫ���Բ������ʹ�� ����GSoC��Ŀ��һ�׶��д�����΢��׼��ܶԸĽ����� �������������������ͬ���õĻ�׼���Ա��������ɫ����2.13�е�ȫ�ֽ�ɫ��ȣ��� 500 ��ȫ�ֽ�ɫ���ԣ�Ȩ�޼����ٶ���߿� 934x �����߱��������һЩ���ܸĽ������ļ��н�ɫ�롰��ɫ���ԡ�����Ŀ��ɫ���бȽϣ����Զ��� 150 ���û���ʵ������֯����������ļ����е� 250 ����Ŀ���е�Ȩ�޼�飬��������ҵ���ٶ�����˽��� 15 �����������ڴ˴��鿴��׼�ͽ���Ƚ� ��

## Jenkins������Ϊ����֧��
�ò��֧�� Jenkins �ġ��������á����ܣ����������ͨ�� Web UI ��������Ȩ�ޡ�YAML����������ʾ��

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
## ����Swagger֧�ֵ�REST API
�ò���ṩ REST API ������ͨ�� Swagger.json ������� OpenAPI �淶�Ľ�ɫ���������� SwaggerHub ��ǩ�� Swagger API  ��SwaggerHub �ṩ�˶������ԵĴ�����������ز������������н�������������ʹ�� curl   �������в鿴һЩʾ������

!(./swagger.png)
!(./swagger2.png)

