# django를 사용해 게시판 만들기

지난 포스팅에 이어서 django를 사용해 게시판 만들기를 이어가보겠습니다.



## I. application 설정

지난 포스팅에서 bash에 다음과 같은 명령어를 입력해서 어플리케이션을 생성했습니다.

```bash
python manage.py startapp board
```

django에서 어플리케이션이란 **실제 서비스에서 작동하게 될 기능**들이라고 설명드렸습니다. 또한, django에서 manage.py는 **프로젝트 전반적인 관리**를 맡는 파일이라고도 말씀드렸습니다.

두 가지를 통해 위 명령어를 해석하면 **manage.py를 실행시켜 board라는 어플리케이션을 생성해라**라는 의미로 설명드릴 수 있습니다.



어플리케이션을 생성했으면, 프로젝트에게 우리가 어떤 어플리케이션을 생성했는지 **등록**해야 합니다. 마치 출생신고를 하는 것처럼요.

프로젝트 설정을 관리하는 settings.py에 들어가면 **INSTALLED_APP**이라는 리스트를 볼 수 있습니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/newapp.png)

위 리스트는 현재 django에 설치된 어플리케이션들의 리스트를 보여줍니다. django.contrib.~로 시작하는 어플리케이션들은 프로젝트를 생성하면 기본적으로 설치되는 어플리케이션들입니다. 마지막 줄에 , 후 우리가 생성한 **어플리케이션 이름**인 `board`를 입력하여 프로젝트에게 우리가 생성한 어플리케이션을 등록합니다.





## II. 기능 구현

어플리케이션을 등록했으니 실제로 작동할 기능을 구현할 차례입니다.

지난 포스팅에서 **views.py**가 실제 작동할 기능을 담고 있다고 설명드렸습니다. 우리는 앞으로 views.py에 함수를 정의하며 서비스에서 작동할 기능을 만들어 나갈것입니다.

기능을 구현하기에 앞서 templates 파일을 먼저 설정하겠습니다.

bash창에서 다음 2개의 명령어를 입력해줍시다.

```bash
mkdir -p board/templates/

touch board/templates/hello.html
```

mkdir은 board 어플리케이션 내에 templates라는 폴더를 생성하고, touch는 templates 안에 hello.html이라는 파일을 생성하라는 의미입니다. 여기서 주의할 점은 파일명은 어떤것이어도 html문서이기만 하면 상관없지만, **templates 폴더 명은 지켜줘야 한다**는 것입니다.



먼저, 가장 간단한 'Hello world'를 띄우는 기능부터 구현해보겠습니다.

views.py와 hello.html 문서에서 다음의 코드를 입력합니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/viewshello.PNG)

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/tempehello.PNG)

해당 코드는 python이 실행시킬 hello라는 기능과 사용자에게 제공할 hello.html 문서를 생성했음을 의미합니다.



기능을 구현했으면 bash창에서 다음의 명령어를 입력하여 서버를 실행시킬 수 있습니다.

```bash
python manage.py runserver
```

manage.py에게 서버를 실행시켜라 라는 의미이고, bash창에 다음과 같은 설명이 출력되는 것을 볼 수 있습니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/runserver.PNG)

여기서 주목해야 할 점은 http://127.0.0.1:8000/ 부분입니다. **127.0.0.1** 은 우리의 **로컬컴퓨터(이제부터는 서버컴퓨터라고 말하겠습니다)의 주소**를 의미하고, **8000**은 해당 **서비스가 제공되는 포트 번호**를 의미합니다. 즉, 누군가가 **서버 컴퓨터**의 **8000번 포트**로 접속하면 실행되고 있는 서비스를 제공받을 수 있다는 것입니다. 해당 주소로 접속하면 다음과 같은 화면을 볼 수 있습니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/djangomain.PNG)

위 페이지는 django 메인페이지 입니다. 여러분이 생성한 프로젝트가 해당 주소에서 작동하고 있음을 확인할 수 있습니다. 그런데 우리가 만든 hello라는 기능은 어디에 가야 볼 수 있을까요?

해당 서비스를 보기 위해 하나의 파일을 더 조작해보겠습니다.



## II. urls

지난 포스팅에서 urls.py가 url과 우리가 만든 기능을 연결해주는 역할을 한다고 설명드렸습니다. urls.py에서 다음의 코드를 입력하면 우리가 생성한 기능과 url이 연결됩니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/urlhello.PNG)

urls.py에 우리가 구현한 기능이 어디 있는지를 알려줘야 합니다.

```python
from board import views
```

해당 코드는 어플리케이션 **board**의 **views**에 가면 우리가 생성한 기능들이 있다는 것을 알려줍니다.

```python
path('hello/', views.hello)
```

path는 url과 우리가 생성한 기능을 연결하는 함수입니다. 사용자가 **\<서버 컴퓨터 주소>/hello/** 라는 url로 접속하면 views에 있는 hello 기능을 실행하도록 연결합니다.

위 설정이 끝나고 다시 브라우저로 돌아가  **\<서버 컴퓨터 주소>/hello/**로 접속하면 hello world가 띄어진 창을 볼 수 있습니다.

![](C:\Users\student\Desktop\django 정리\static_2\helloworld.PNG)

## III. MTV 구조

여기서 한번만 더 MTV 구조를 설명하고 넘어가겠습니다.

지난 포스팅에서 다음과 같은 단계로 MTV 패턴을 설명드렸습니다.

1. 사용자가 자료를 요청
2. 필요시 데이터베이스에서 자료를 찾음
3. views에서 사용자의 요청에 따라 정보를 제작
4. 결과를 html 문서에 담아 사용자에게 제공

여기에 하나더 urls.py가 views에서 정의한 기능과 사용자가 요청한 url을 연결시켜준다는 점을 추가 해봅시다.

그럼 작동 방식이 다음과 같이 정의될 수 있습니다.

1. 사용자가 자료를 요청
2. **urls.py가 url에 해당하는 views의 기능으로 연결**
3. 이후 동일...

그림으로 표현하면

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/basic-django.png)

[출처: https://developer.mozilla.org/ko/docs/Learn/Server-side/Django](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django)

과 같이 나타낼 수 있습니다.

Http, 즉 웹 서비스가 작동하는 방식은, 사용자가 url(자료가 있는 주소, 위치)을 요청하면 해당 위치에 있는 자료를 서버컴퓨터가 제공하는 방식입니다.

만약 어떤 사용자가 우리가 정의한 hello 라는 서비스를 받아보고 싶다고 한다면 대략적으로 우리의 서버컴퓨터 주소 + hello 라는 url을 사용하여 접속해야 할 것입니다.

django는 서버컴퓨터 주소까지 미리 생성하여 사용자에게 제공합니다. 위에서 본 django 메인페이지가 그것입니다. 추가적으로, **hello라는 기능에 접근하려면 hello라는 기능과 연결된 url 주소로 접근해야 하며, 그 연결을 urls.py가 담당합니다.** 

그럼 우리가 정의한 hello라는 기능에 접근해보겠습니다. 우리는 이미 **II**번에서 **hello**기능과 **hello/** url을 urls.py를 통해 연결시켰습니다. 서버컴퓨터주소 + hello/인 http://127.0.0.1:8000/hello/로 접근해보겠습니다.

접근하면 다음과 같이 우리가 정의한 기능이 브라우저를 통해 출력되는 것을 볼 수 있습니다.

![](C:/Users/student/Desktop/django%20%EC%A0%95%EB%A6%AC/static_2/hello.PNG)

이상으로 django가 MTV패턴 위에서 어떻게 작동하는지 알아보았습니다. MTV패턴은 django의 작동 원리이므로 django를 사용하겠다고 하면 반드시 알아야 하는 개념입니다. views와 urls.py에 대해 다뤘으니 다음 포스팅에서는 Template의 기능에 대해 다뤄보겠습니다.

