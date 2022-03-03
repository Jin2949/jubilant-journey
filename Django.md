<img src=".\화면 캡처 2022-03-02 103324.png" alt="image" style="zoom: 25%;" />

클라이언트 서버 , 요청하고 응답하는 구조이다.

클라이언트(웹 브라우저-크롬)

서버 구축 프레임워크(장고) - 프레임워크란? 함수, 등 여러가지 기능을 이미 구현해놓은 툴



### 프레임워크 아키텍쳐

MVC Design Pattern(model-view-controller)

장고는 MTV Pattern이라고 한다.(model-template-view)로 지정했다.

Model - 데이터베이스 기록 관리

Template - 내용을 보여주는 데 사용(presentation)

View - Model데이터에 접근, Template에 응답 서식 설정



### 장고 초기설정

1. 가상환경 생성 및 활성화

   ```python
   #새폴더에서 git bash
   python -m venv venv
   source venv/Scripts/activate
   pip list #새로운 환경인지 확인
   #vscode에서 켜서
   python select interpreter - python recommanded누르기
   ```

2. django 설치

   ```python
   pip install django==3.2.12
   pip list #변경사항 확인
   ```

3. 프로젝트 생성

   ```python
   django-admin startproject firstpjt .
   ```

4. 서버 켜서 로켓 확인하기(서버활성화)

   ```python
   python manage.py runserver #<command>를 사용한다. : runserver는 명령어
   ```

5. 앱 생성 (articles)

   ```python
   python manage.py startapp articles	#명령어 사용
   ```

6. 앱 등록 (INSTALLED_APPS)

   ```python
   #settings.py에서 INSTALLED APPS에 등록한다.
   'articles', 
   ```

### 서버 반응순서

서버에 요청을 /admin/을 보낸다.

서버의 urls.py에서 받아서 path('admin/')의 함수가 호출이 되었다. - 관리자페이지 주면 된다

우리가 사용하는 뒤로가기, 하이퍼링크도 다 서버에 요청을 하면 응답을 주는 것이다.

### trailing comma

바로 작성할 수 있도록 생산성 높인 모양

### apps

앱 폴더 안에 templates으로 만들어줘야 한다. 내부경로가 정해져 있다.

```python
#app/templates 로 만들어줘야한다.
templates/index.html #을 구성해서 html안에서 페이지 요청 만들 수 있다.
```

프레임워크 사용

```python
#urls.py 에 path('index/',views.index)	view함수 실행하도록 만듦
#app에서 views.py 에 
def index(request):
    return render(request, 'index.html')
```

### 코드의 작성순서

URL ---> VIEW ---> TEMPLATE 순서로 만들도록 한다.
데이터의 흐름순서이다.

### DjangoTemplate Language(DTL)

1. Variable(변수)

   ```python
   #view에 {'name':'Alice'}를 넣어놓고
   #연결된 Template에서 {{name}} 을 사용하여서 변수를 지정할 수 있다.
   context = {} 로 변수지정하고 안에 넣고싶은거 넣어도 아래에서 작동함.
   render(request, 'greeting.html', context)
   
   def greeting(request):
       foods = ['apple', 'banana', 'coconut',]
       info = {
           'name': 'Alice',
       }
       context = {
           'foods': foods,
           'info': info,
       }
       return render(request, 'greeting.html', context)
   #위에처럼 views.py에서 선언후
   #html에서 아래처럼 사용가능하다.
     <p>안녕하세요 저는 {{ info.name }}입니다.</p>
     <p>제가 가장 좋아하는 음식은 {{ foods }}입니다.</p>
     <p>저는 사실 {{ foods.0 }}을 가장 좋아합니다.</p>
   ```

2. Filters

   ```python
   <p>안녕하세요 저는 {{ info.name|lower }}입니다.</p>
   #|lower를 써서 Alice가 alice가 된다.
   #django의 built in tag & filter (문서에서 찾아서 쓴다)
   ```

3. Tags

   ```python
   변수보다 복잡한 일들을 수행
   일부 태그 {% if %}{% endif %}
   
   for태그
   for-forloop태그 : 숫자붙여줌 리스트에
   for-empty태그 : 비었을 때, 비었을때 출력할 문장 선정
   
   if태그
   length filter활용
   
   템플릿들에 부트스트랩을 입히려면 상속개념을 사용한다.
   재사용성에 초점, 재정의 할 수 있도록 함. (CDN적용)
   두개의 태그가 필요하다.(extends, block 태그)
   
   템플릿 상속
   settings.py- TEMPLATES = [{'DIRS': [BASE_DIR / 'templates',],] 를 새로 추가한다.
   여기서 BASE_DIR 은 현재 장고프로젝트 시작점이다.
   templates파일은 pjt와 app의 동일한 위치에 만든다(상속할때)
   
   base.html에 navbar삽입시키고
   index.html에는 extends사용
   {% extends 'base.html' %}
   {% block content %}
     <h1>안뇽하세요!</h1>
     <p>안녕하세요 저는 {{ info.name }}입니다.</p>
   {% endblock content %}
   
   base도 많아진다면 nav.html을 따로 뜯어다가
   include태그
   base.html에 
   {% include 'nav.html' %} 를 사용하면된다.
   ```

4. comments

   ```python
   comments는 #과 같은 기능으로 사용하며 주석처리할때 사용함.
   ```

   

