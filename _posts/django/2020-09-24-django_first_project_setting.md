---
title: "Django 프로젝트 세팅"
categories: 
  - "Django"
---

## 소개
* Django 초기 세팅 과정에 대해 소개 한다.
* 본 게시글은 윈도우 bash shell 기준으로 작성 되었다.
* <a target="_blank" href="https://github.com/eunche/blog.django.example-project">Github Link</a>

<br>

# 목차
> 1. <a style="text-decoration-line:none; color: black;" href="#1-가상환경-설정">가상환경 설정</a>
> 2. <a style="text-decoration-line:none; color: black;" href="#2-가상환경-안에-django-설치">가상환경 안에 Django 설치</a>
> 3. <a style="text-decoration-line:none; color: black;" href="#3-django-project-생성">Django project 생성</a>
> 4. <a style="text-decoration-line:none; color: black;" href="#4-django-app-생성등록">Django app 생성/등록</a>
> 5. <a style="text-decoration-line:none; color: black;" href="#5-url-설정">url 설정</a>
> 6. <a style="text-decoration-line:none; color: black;" href="#6-templates-설정">templates 설정</a>
> 7. <a style="text-decoration-line:none; color: black;" href="#7-static-설정">static 설정</a>
> 8. <a style="text-decoration-line:none; color: black;" href="#8-커스텀-user-모델-설정선택">커스텀 User 모델 설정(선택)</a>
> 9. <a style="text-decoration-line:none; color: black;" href="#9-gitignore-추가">.gitignore 추가</a>

<br><br><br>

# 1. 가상환경 설정
* (경로 : example-project)

```bash
$ python -m venv venv
```

>  "python -m venv 가상환경명" 으로, venv라는 이름의 가상환경을 만들었다   

```bash
$ source venv/Scripts/activate
```

> 가상환경을 켜주었다


<br><br><br>

# 2. 가상환경 안에 Django 설치

```bash
$ pip install django
```

> "pip install django==2.1.1"과 같이 특정버전을 지정하지 않으면 최신버전의 django가 설치된다

<br><br><br>

# 3. Django project 생성

```bash
$ django-admin startproject config .
```

> "django-admin startproject 프로젝트명" 에 추가로 " ."이 붙어 현재 경로인 "example-project"폴더 내에 파일이 생성되게 된다

##### 프로젝트 구조

<img src="/assets/images/django/2020-09-24-django_first_project_setting/directory1.JPG">

<br><br><br>

# 4. Django app 생성/등록
* (users/posts 두개의 app만 예시로 듦)

### App 생성

```bash
$ python manage.py startapp users   
$ python manage.py startapp posts
```

>  "python manage.py startapp 앱이름" 으로, 두개의 app을 생성했다

### App 등록

* (config/settings.py의 INSTALLED_APPS 부분)

```python
...
INSTALLED_APPS = [
    'users.apps.UsersConfig',
    'posts.apps.PostsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

> 위의 INSTALLED_APPS 부분에서

```python
    'users.apps.UsersConfig',
    'posts.apps.PostsConfig',
```

> 이 두줄을 추가 해주었다   
> <em>(예 : users/apps.py의 UsersConfig 클래스를 등록)</em>

<br><br><br>

# 5. url 설정
* (config/urls.py)

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('post/', include('posts.urls')),
    path('user/', include('users.urls')),
]
```

> * 프로젝트 폴더(config)에 있는 한 urls.py에서 모든 path를 관리하게 되면 관리가 어려워지므로, App 별로 urls.py를 새로 만들어서 관리하는 include 방식으로 구성했다
> * 현재 users, posts app에 urls.py 파일이 없으므로 만들어주자

<br>

* (users/urls.py)

```python
from django.urls import path
from . import views

app_name = 'users'

urlpatterns = []
```

<br>

* (posts/urls.py)

```python
from django.urls import path
from . import views

app_name = 'posts'

urlpatterns = []
```

<br><br><br>

# 6. templates 설정

* (config/settings.py의 TEMPLATES 부분)

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, "templates")],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

> 위의 TEMPLATES 부분에서

```python
'DIRS': [os.path.join(BASE_DIR, "templates")],
```

> 'DIRS' 부분만 추가하여, "example-project/tamplates" 경로를 html 파일들이 있는 경로로 설정하였다

### Django 최신버전일 경우

두가지 선택지가 있다   

(1) settings.py 상단에 "import os"를 추가    

```python
import os
```

<br>

(2) 혹은 DIRS부분을 변경
```python
'DIRS': [BASE_DIR / "templates"],
```
> <em>(참고 : Django 최신 버전에선, 경로들이 pathlib의 Path가 쓰였기 때문에 위와 같이 설정이 가능하다)</em>

##### 프로젝트 구조

<img src="/assets/images/django/2020-09-24-django_first_project_setting/directory2.JPG">

<br><br><br>

# 7. static 설정

* (config/settings.py의 최하단 부분)

```python
STATIC_URL = "/static/"
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static_local")]
STATIC_ROOT = os.path.join(BASE_DIR, "static")
```

* <strong>STATIC_URL</strong> : 웹 페이지에서 사용할 정적 파일의 최상위 URL 경로   
* <strong>STATICFILES_DIRS</strong> : 개발 단계에서 사용하는 정적 파일들이 위치한 경로   
* <strong>STATIC_ROOT</strong> : collectstatic 명령어를 통해 Django 프로젝트에서 사용하는 모든 정적 파일을 한곳에 모아 넣는 경로(배포 시 사용하기 위해)   

### 참고
> * <em>Path를 이용한 방법으로 경로를 구성할 수도 있지만, 현재 예제에서는 오류가 난다면 "import os"를 최상단에 추가해 주자</em>
> * <em>static_local, static으로 폴더명을 구분하는 것보단 settings.py를 local/deploy 환경으로 구분하여 만들어주는 게 더 좋은 방법인 것 같다</em>

##### 프로젝트 구조

<img src="/assets/images/django/2020-09-24-django_first_project_setting/directory3.JPG">

<br><br><br>

# 8. 커스텀 User 모델 설정(선택)

> Django에서 기본 제공하는 User 모델이 있지만, 커스텀 User 모델을 사용할 수 있다   
> 첫 migrate 이전에 커스텀 User 모델을 설정해야 오류가 나지 않기 때문에, Django 초기 세팅 과정 글에 넣게 되었다

### 관련 포스트

* <a target="_blank" href="https://eunche.github.io/django/django_custom_user_model/">Django 커스텀 User 모델</a>

<br><br><br>

# 9. gitignore 추가

아래 링크에 들어가서 작성돼 있는 코드를 복사 하여 .gitignore 파일에 추가한다   

* <a target="_blank" href="https://gist.github.com/santoshpurbey/6f982faf1eacdac153ffd86a3a694239">.gitignore 링크</a>

<br><br><br>

# 최종 프로젝트 구조

<img src="/assets/images/django/2020-09-24-django_first_project_setting/directory4.JPG">
