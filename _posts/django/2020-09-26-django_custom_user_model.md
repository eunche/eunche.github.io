---
title: "Django 커스텀 User 모델"
categories: 
  - "Django"
---

## 소개
* Django에서 기본 제공하는 User 모델이 있지만, 커스텀 User 모델을 사용할 수 있다.
* 첫 migrate 이전에 커스텀 User 모델을 설정해야 오류가 나지 않기 때문에 유의하자.
* 본 게시글은 윈도우 bash shell 기준으로 작성되었다.

<br>

# 모델 등록

* (settings.py의 최하단에 작성)

```python
AUTH_USER_MODEL = "users.User"
```

> 'users'라는 Django app 안에 있는 'User' 모델을 등록한다는 뜻이므로, 모델을 작성해 주자

<br><br><br>

# 모델 작성

* (users/models.py)

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

> * User 모델의 비어있는 'pass' 부분에 추가하고 싶은 필드나, 메소드 등을 추가해 주자
> * 상속받는 'AbstractUser'모델의 필드는
>  username ,first_name ,last_name ,email ,is_staff   
> 등이 있다

<br><br><br>

# Migration

```bash
$ python manage.py makemigrations   
$ python manage.py migrate
```

<br><br><br>

#### 참고 자료
* 본 내용은 <a href="https://nomadcoders.co/airbnb-clone" target="_blank"><strong>'노마드 코더 에어비앤비 클론 코딩'</strong></a> 강의 내용을 토대로 작성되었습니다