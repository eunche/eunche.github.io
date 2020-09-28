---
title: "Github로 정적 웹 페이지 호스팅 하기"
categories: 
  - "Deploy"
---

# Github란?

> 협업 툴인 Git을 사용하는 프로젝트를 지원하는 웹호스팅 서비스이다.
> 프로젝트를 Github에 업로드할 수 있고, 정적 웹 페이지를 무료 호스팅 할 수 있다.

<br><br><br>

# 정적 웹 페이지란?

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/static_web_page.png">

> 웹 서버에 미리 저장된 파일들(HTML, img, Javascript 파일 등)이 그대로 사용자에게 전달되어 보여지는 웹 페이지이다.

<br><br><br>

# 호스팅 과정

## 1. Github 가입

* <a href="https://github.com/" target="_blank">https://github.com/</a>

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/github_signup.JPG">

> 위 항목을 다 채운 후 가입해 주자

<br><br>

## 2. 저장소 생성

* <a href="https://github.com/new" target="_blank">https://github.com/new</a>

메인 화면에서 <strong style="color:#2ea44f">New</strong> 버튼을 누르거나, 위 링크로 들어가자


> <strong>-Repository name : 프로젝트 이름</strong>   
> <em>("github아이디.github.io"로 저장소를 만들면, 별도의 설정 없이 이 블로그와 같은 "https://github아이디.github.io/"의 형태로 웹 페이지가 생성된다)</em>
> 
> <strong>-Description : 프로젝트 설명</strong>   
> 
> <strong>-Public/Private : 전체공개/비공개 설정</strong>   
> <em>(Private으로 설정하면, Github 호스팅 서비스를 사용할 수 없다)</em>

<br><br>

## 3. 프로젝트 업로드

* <em style="color:gray;">git CLI(터미널을 이용하는 방식)를 통해 프로젝트를 github 상에 업로드할 수 있지만, git/github 사용에 익숙하지 않은 분들을 고려하여 github에서 직접 파일을 업로드하는 방식을 선택했다<em>

<br>

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/upload.png">

> 1. <strong style="color:#0366d6">uploading an existing file</strong> 클릭   
> 2. 파일 드래그 or 파일 선택을 통해 파일들을 업로드   
> 3. <strong style="color:#2ea44f">Commit changes</strong> 클릭

<br><br>

## 4. Github Pages 설정

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/setting.png">

> <strong>Settings</strong> 항목을 클릭

<br>

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/github_pages.png">

> <strong>None</strong> 을 클릭하여 master(or base)로 변경

<br>

<img src="/assets/images/2020-09-28-github_static_web_page_hosting/result_page.png">

> 호스팅에 소요되는 0~2분 후에 링크를 들어가면, 웹 페이지가 정상적으로 작동하게 된다

<br><br><br>

# 오류 원인

> 위 예시대로 진행했지만, 페이지가 제대로 작동하지 않을수 있다. 해결 사례로는, '파일명.PNG' 인데 코드상에서는 '파일명.png'로
> 작성하여 로컬에서는 작동하나 호스팅은 제대로 되지 않은 경우가 있었다. 그 외에도 HTML 문법은 잘 지켰는지, 링크 참조는 잘 되었는지
> 등을 꼼꼼히 확인해 주자.

<br><br><br>

#### 참고 자료
* <a href="https://ko.wikipedia.org/wiki/%EA%B9%83%ED%97%88%EB%B8%8C" target="_blank">위키백과 깃허브</a>
* <a href="https://titus94.tistory.com/4" target="_blank">Titus `열정의 공간 블로그</a>
* <a href="https://hogni.tistory.com/75" target="_blank">빠른손김참치 블로그</a>