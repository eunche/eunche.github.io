---
title: "AWS EB(Elastic Beanstalk)에 Django 배포"
categories: 
  - "Deploy"
---

<a href="https://eunche.github.io/deploy/aws_eb_django_deploy/">(1) - AWS EB(Elastic Beanstalk)에 Django 배포</a><br>
(2) - AWS EB(Elastic Beanstalk) Django 배포 오류 해결<strong>(포스팅 예정)</strong><br>
(3) - AWS EB(Elastic Beanstalk) Django와 RDS(PostgreSQL) 연결<strong>(포스팅 예정)</strong><br>
(4) - AWS EB(Elastic Beanstalk) Django Static/Media파일 AWS S3에 분리<strong>(포스팅 예정)</strong><br>

<br><br><br>

## 소개
* Django 2.1.5 버전 사용
* Python3 venv 모듈 사용
* Windows 10 환경
* bash shell 기준으로 작성
* <a target="_blank" href="https://github.com/eunche/blog-code/tree/master/aws-eb-django-deploy">Github Link</a>

<br><br><br>

# AWS Elastic Beanstalk 이란?

> * 배포 과정을 잘 알지 못해도 웹 애플리케이션을 신속하게 배포하고 관리하게 해주는 서비스이다.
> * Elastic Beanstalk에 대한 추가 비용은 없고, EC2(클라우드 컴퓨팅 서비스)와 같은 기본 리소스에 대한 비용만 지불하면 된다.

<br><br><br>





# 배포 과정

## 1. Django 프로젝트를 github에 업로드

<em style="color:gray;">- AWS EB를 통해 배포 할때, git을 사용하게 되면 최신 커밋이 배포된다.</em>
<br>
<em style="color:gray;">&nbsp;(변경사항이 생길 때마다 commmit을 작성해 주어야 한다)</em>

```bash
$ pip install django==2.1.5
```

> Elastic Beanstalk Python 3.6 플랫폼과 호환 가능한 최신의 Django 버전은 2.1이기 때문에 Django 버전을 알맞게 설치해 주자

<br>

#### (프로젝트 구조)
<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/first_project.JPG">

<br><br><br>


## 2. Elastic Beanstalk CLI 설치

* 특정 프로젝트에서만 사용될 게 아니므로, 가상환경이 꺼진 상태에서 설치해 주었다

```bash
$ pip install awsebcli --upgrade --user
```

<br><br><br>


## 3. requirements.txt 작성

<em style="color: gray ;font-weight:normal;">(가상환경이 켜진 상태)</em>

```bash
$ pip freeze > requirements.txt
```
> EB를 통해 배포될 때 EC2에 설치될 패키지를 requirements.txt에 작성하였다

<br><br><br>


## 4. WSGIPath 설정을 위한 구성 파일 추가

* (.ebextensions/django.config)

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/wsgipath.png">

<br><br><br>


## 5. IAM 사용자 추가

* <strong>IAM 사용자</strong> : (Django)애플리케이션에 AWS 계정의 리소스 접근을 권한을 받은 것

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/iam1.png">

<br><br>

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/iam2.png">

<br><br>

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/iam3.png">

> <strong>AdminstratorAccess</strong>를 설정하면, 모든 권한을 갖게 된다

<br>

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/iam4.png">

> IAM 사용자를 만들게 되면 생성되는 액세스 키 ID와 PASSWORD는 절대 유출되어선 안되며, 따로 작성해두거나 .csv파일로 보관하자

<br><br><br>


## 6. EB Application 생성

* <strong>EB Application</strong> : 웹 앱을 실행하는 환경, 웹 앱의 소스 코드 버전, 저장된 구성, 로그 및 Elastic Beanstalk를 사용하는 동안 생성한 기타 데이터의 컨테이너 역할

```bash
$ eb init
```

<br>  

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/region.png">

> 서비스가 될 지역을 10번, Seoul로 고른 상태

<br><br>

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/createapp.png">

> * EB Application을 생성
> * IAM 사용자가 등록돼있지 않다면, 아까 받았던 access ID, PASSWORD를 입력한다

<br>

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/pythonset.png">

> * 본 예제에서는 꼭 'Python 3.6 ... Amazon Linux' (2)번 옵션으로 선택해 주자

<br><br>


## 7. EB Environment 생성

* <strong>EB Environment</strong> : 애플리케이션 버전을 실행 중인 AWS 리소스 모음(EC2, LoadBalancer, Security Group 등)

```bash
$ eb create django-eb-env
```

> eb create "\<환경 이름\>"

<br>

```bash
$ eb status
```

<img src="/assets/images/deploy/2021-02-01-aws_eb_django_deploy/status.png">

> 도메인 별명(CNAME)을 받아 ALLOWED_HOST에 추가해 주자

<br>

* (settings.py)

```
ALLOWED_HOSTS = ['127.0.0.1', 'django-eb-env.eba-t23rma5a.ap-northeast-2.elasticbeanstalk.com']
```

<br><br><br>


## 8. 애플리케이션 배포

* 배포 준비가 다 되었다면, 변경 사항들을 전부 commit 해주고 아래 명령어를 입력해 주자

```bash
$ eb deploy django-eb-env
```

> eb deploy "\<환경 이름\>"

<br>

```bash
$ eb open
```

> 최종적으로, AWS EB를 통해 배포된 Django 웹페이지를 확인할 수 있다


<br><br>


#### 참고 자료
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/Welcome.html" target="_blank">AWS EB - EB란?</a>
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb3-cli-git.html" target="_blank">AWS EB - EB CLI와 Git 사용</a>
* <a href="https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-django.html" target="_blank">AWS EB - Elastic Beanstalk에 Django 애플리케이션 배포</a>
* <a href="https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/introduction.html" target="_blank">AWS IAM</a>

