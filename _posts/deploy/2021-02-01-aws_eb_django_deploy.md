---
title: "AWS EB(Elastic Beanstalk)에 Django 배포하기"
categories: 
  - "Deploy"
---

## 소개
* Django 2.1.5버전 사용
* Python3 venv 모듈 사용
* Windows 10 환경
* bash shell 기준으로 작성

<br><br><br>

# AWS Elastic Beanstalk이란?

> * 배포 과정을 잘 알지 못해도 웹 애플리케이션을 신속하게 배포하고 관리하게 해주는 서비스이다.
> * Elastic Beanstalk에 대한 추가 비용은 없고, EC2(클라우드 컴퓨팅 서비스)와 같은 기본 리소스에 대한 비용만 지불하면 된다.

<br><br><br>





# 배포 과정

## 1. Django 프로젝트를 github에 업로드

<em style="color:gray;">- AWS EB를 통해 배포 할 때, git을 사용하게 되면 최신 커밋이 배포 된다.</em>

```bash
$ pip install django==2.1.5
```

> Elastic Beanstalk Python 3.6 플랫폼과 호환 가능한 최신의 Django 버전은 2.1이기 때문에 Django 버전을 알맞게 설치 해주자

<img src="/assets/images/2021-02-01-aws_eb_django_deploy/first_project.JPG">


<br><br><br>


## 2. Elastic Beanstalk CLi 설치

```bash
$ pip install awsebcli --upgrade --user
```

<br><br><br>



#### 참고 자료
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/Welcome.html" target="_blank">AWS EB - EB란?</a>
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb3-cli-git.html" target="_blank">AWS EB - EB CLI와 Git 사용</a>
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-django.html" target="_blank">AWS EB - Elastic Beanstalk에 Django 애플리케이션 배포</a>
