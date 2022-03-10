### 장고프로젝트 생성

```python
#git bash
python -m venv venv
source venv/Scripts/activate
pip install -r requirements.txt

#vscode/git bash
django-admin startproject firstpjt .
python manage.py startapp articles
#settings articles앱등록
```

```python
#models.py 접속
class Article(models.Model):
    title = models.CharField(max_length=30)   
    content = models.TextField()
    #Model 조작(생성, 수정, 삭제)하는 경우 마이그레이션을 생성하고 적용한다. 
    #git bash : python manage.py makemigrations -- python manage.py migrate
    #migrate 해야 admin auth contenttypes sessions기본기능 DB를 적용한다.

    #python manage.py sqlmigrate articles(app_name) 0001(migration_number)
    #우리의 코드가 실제 DB에 전달되는 SQL문을 확인할 수 있다. (ORM이 변환)

    #miagration이 적용이 됐는지 여부를 확인하는 명령어이다.
    python manage.py showmigrations
    
    #행이 최초 생성될때 날짜랑 시간을 자동으로 입력되도록 함.    
    created_at = models.DateTimeField(auto_now_add=True)     
    #최초값이 들어가는 것이 아니라 수정할때마다 수정일을 적어주는 것
    updated_at = models.DateTimeField(auto_now=True)     
	
    #여기서 python manage.py makemigrations 를 하면 1,2번 선택하는게 나온다, 1번은 기본값 넣어주기, 2번은 내가 다시 바꿔서 넣어줄게 설정안된 값에는 어떻게 설정해줄건지 다음 python manage.py migrate해준다.
    
    #ORM사용하기(DB API)
    #python manage.py shell을 켜서 ORM을 사용해본다.(장고기능 다 사용)

    #articles.models에 있는 Articles클래스 가져와라고 명령 쉘을 켜는 것임.
    #from articles.models import Article
    
    #Article.objects.all()      쿼리셋으로 결과를 보여준다.
    
    #라이브러리 설치
    이유 : 장고쉘이 불편하다, 모델을 이용해 DB에 접속하는 건데 모델이 여러개면 하나씩 from~import 쓰기가 힘들다. 그래서 pip install django-extentions, pip install ipython 설치 후에 requirements.txt에 추가하기 위해서 pip freeze > requirements.txt 를 해준다. 이후, project의 settings에 가서 INSTALLED_APPS에 추가해준다.(django_extentions)
	
    #article의 제목만 뜨게하는법(str열)
    #models.py 에 추가한다.
    def __str__(self):
        return self.title
    
    #데이터 추가하는 방법 3가지(create방법)
    첫번째 방법
    article = Article()
    article.title = ''
    article.contnet = ''
    article.save()
    
    두번째 방법
    article = Article(title='', content='')
    article.save()
    
    세번째 방법
	Article.objects.cerate(title='',content='')
    
    #Read하는법
    def __str__(self):
    return self.title #를 쓰면 조회할때, 제목을 조회할 수 있다.
    #Article.objects.all() 전체 데이터 조회함.
    1번데이터 조회
    #article_1 = Article.objects.all()[0]
    #article = Article.objects.get(id=1)
    오류뜸!
    #article = Article.objects.get(title="첫번째 글")
    title이 첫번째 글인게 2개여서 get은 1개만 가져오기 때문에 
    #filter배우기   
    articles = Article.objects.filter(title="첫번째 글")
    #contains와 같은 것들을 field lookup으로 공식문서에서 보면 된다.
    articles = Article.objects.filter(title__contains="첫")

    #update하는법
    #article에 Article의 4번째거를 가져온다. article = Article.objects.get(pk=4)
    #article.title = "네번째 글"
    #title을 바꿔주고 save하면 update가 된다.
    
    #delete하는법
    #그리고 article.delete()를 하면 Article에서 article이 지정한게 사라진다. 
    
    #admin페이지의 로그인 설정
    #python manage.py createsuperuser
    #아이디 비번 설정하고 

    articles의 admin.py에서
    #from .models import Article
    #admin.site.register(Article)를 하면 서버에서 관리자페이지에서 DB접속할 수 있다.
```

## urls.py 경로 설정하기

 ```python
 articles의 urls.py로 url처리 넘기고, index/ 와 매칭이 되면 index.html(render해서 보여주기)
 articles에 urls.py(만들기)
 원래 프로젝트의 urls.py 코드에서 필요한 부분가져오기
 from django.contrib import admin
 from django.urls import path
 
 urlpatterns = [
     path('admin/', admin.site.urls),
 ]
 # 위에 코드를 아래처럼 바꿔줌.
 from django.urls import path
 from . import views
 urlpatterns = [
     path('index/', views.index),
 ]
 
 #그리고 view에가서 
 def index(request):
     return render(request, 'articles/index.html')
 
 templates/articles만들고 index.html만들어주기
 
 #프로젝트 urls에서 경로 재설정해주기(include설정)
 from django.contrib import admin
 from django.urls import path, include
 
 urlpatterns = [
     path('admin/', admin.site.urls),
     path('articles/', include('articles.urls')),
 ]
 ```

```python
html기본구조 만들어주기
기본위치에 templates/base.html 넣고 부트스트랩 설정
  <div class="container">
    {% block content %}
    {% endblock content %} #body에 추가

DIRS :[BASE_DIR / 'templates']
    
index.html가서
{% extends 'base.html' %}
{% block content %}
{% endblock content %}
```

## DB데이터 가져오기

```python
만들어놓은 DB view로 가져오기
#views.py
from .models import Article
urlpatterns = [
    path('index/', views.index),
]

def index(request):
    articles = Article.objects.all()
    context = {'articles':articles}
    return render(request, 'articles/index.html', context)

#index.html에서는 아래코드로 추가
{% block content %}
  <h1 class="text-center">Articles</h1>
  {% for article in articles %}
    <p>글 번호 : {{ article.id }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <p>글 내용 : {{ article.content }}</p>
    <hr>
  {% endfor %}
{% endblock content %}
```

```python
new페이지 추가하기
#urls.py
path('new/', views.new),
#views.py
def new(request):
    return render(request, 'articles/new.html')
#new.html
#사용자가 입력할 form생성
{% extends 'base.html' %}
{% block content %}
  <h1 class ="text-center">New Article</h1>

  <form action="">
    {% csrf_token %}
    <label for="title">Title : </label>
    <input type="text" name="title" id="title"><br>
#name으로 title이란걸 알려주고 for와 id를 이어서 label을 클릭하면 포커싱이 갈 수 있도록 해준다.
    <label for="content">content : </label><br>
    <textarea name="content" id="content" cols="30" rows="5"></textarea><br>
#textarea는 input과 다르게 상자를 만들어줘서 좀 더 보는게 편하도록 만든다.
    <input type="submit">
#제출버튼 생성
  </form>

  <a href="{% url 'articles:index' %}">목록보기</a>
{% endblock content %}
#index로 돌아가는 버튼
```

## url태그 이용한 app_name등 추가

```python
url태그와 각페이지가 이동할 수 있도록 추가기능
a태그에서 url태그로 만들어줄때 해야함.
#urls.py
from django.urls import path
from . import views

app_name = "articles" #app_name추가

urlpatterns = [
    path('index/', views.index, name='index'),
    path('new/', views.new, name='new'), #neme추가
]
#index.html
{% block content %}
  <h1 class="text-center">Articles</h1>
  <a href="{% url 'articles:new' %}">글쓰기</a>
```

```python
create추가하기
# urls.py
path('create/', views.create, name='create'),
# views.py
def create(request):
    return render(request, 'articles/create.html')
#create.html
{% extends 'base.html' %}

{% block content %}
<h1>나중에 삭제</h1>
<a href={% url 'articles:index' %}>돌아가기</a>
{% endblock content %}

```

```python
new.html에서 form부분 수정
  <form action="{% url 'articles:create' %}">
    #이렇게 하면 제출은 됨. 이 내용을 저장하기 위해서 views.py/create에서 교정

#views.py
#redirect추가
from django.shortcuts import render, redirect

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    article = Article(title=title, content=content)
    article.save()
    return render(request, 'articles/create.html')
```

```python
http method
GET : 기본값, 서버 리소스를 요청할 때 사용 (R)
    QUERY sting으로 보냄
POST : 리소스를 생성, 수정, 삭제할때 사용 (CUD)
    BODY에 숨겨서 보냄
#new.html 에서 POST사용한다.
#GET방식인 경우 http에 내용을 입력하고 게시물을 등록할 수도 있기 때문에 POST사용
#CSRF Token을 만들어서 위에 GET방식의 문제점 보완한다.
#따라서 POST를 사용하면 아래에 csrf_token이 따라온다.
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}

#views.py에서도 POST로 바꿔줌
    # title = request.GET.get('title')
    # content = request.GET.get('content')
    title = request.POST.get('title')
    content = request.POST.get('content')
```

```python
index.html로 가면 views.py의 create에 articles 값이 없어서 글내용이 안나옴.
해결법 : index url로 가라. redirect사용
# redirect 특정위치로 사용자를 보냄
# 원래 함수
return render(request, 'articles/create.html')
# 변경했지만 글내용이 뜨지 않는 함수
return render(request, 'articles/index.html')
# 해결책
return redirect('articles:index')

```

## 상세페이지

```python
글목록에서 내용까지 나오면 안된다. 
상세페이지 만들기(detail)
variable routing사용
#urls.py
path('<int:pk>/', views.detail, name='detail'),
#views.py
def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article':article
    }
    return render(request, 'articles/detail.html', context)
```

```html
#detail.html
#html 주의사항, a태그에서 article.pk주의 url넣기
#post는 바로 실행될 수 있도록 form으로 구성해서 해준다.

{% extends 'base.html' %}

{% block content %}
<h1 class="text-center">DETAIL</h1>
<h3>{{ article.id }}번째 글</h3>
<p>제목 : {{ article.title }} </p>
<p>내용 : {{ article.content }}</p>
<p>작성 시각 : {{ article.created_at }}</p>
<p>수정 시각 : {{ article.updated_at }}</p>
<hr>
<a href="{% url 'articles:index' %}">목록보기</a><br>

{% endblock content %}

#index.html
#글제목 누르면 수정할 수 있는 상세페이지로 갈 수 있도록 만듦.

```

```python
#글 작성하고 만든 detail페이지로 가기
#아래에 article에 하나씩 지정해줘야 나중에 return에서 article.pk와 같은 것들을 사용할 수 있다.
def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    article = Article(title=title, content=content)
    article.save()
    return redirect('articles:detail', article.pk )
```

## delete구현

```python
1번글 2번글로 가야하므로 variable routing을 해줘야함.
#urls.py
path('<int:pk>/delete/', views.delete, name='delete'),
#views에서 POST 방식으로 받아서 삭제할 수 있도록 해야함. 그래서 아래와 같이 if문을 사용해서 지정해줄 수 있도록 한다. GET방식 제거
#views.py
def delete(request,pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect('articles:detail', article.pk)
```

```html
#detail.html 에서 form으로 구현하기, post주의
<form action="{% url 'articles:delete' article.pk%}" method="post">
  {% csrf_token %}
  <button class="btn btn-danger">삭제하기</button>
</form>
```

## edit 구현

```python
#urls.py
path('<int:pk>/edit/', views.edit, name='edit'),
#views.py
def edit(request,pk):
    article = Article.objects.get(pk=pk)
    context = {'article':article}
    return render(request, 'articles/edit.html', context)
```

```html
#new.html이랑 비슷하게 만든다. 
#input에 value넣는거 주의
#edit.html

{% extends 'base.html' %}

{% block content %}
  <h1 class ="text-center">edit</h1>

  <form action="{% url 'articles:update' article.pk%}" method="POST">
    {% csrf_token %}
    <label for="title">Title :</label>
    <input type="text" name="title" id="title" value="{{ article.title }}"><br>
    <label for="content">content : </label><br>
    <textarea name="content" id="content" cols="30" rows="5">{{ article.content }}</textarea><br>
    <input type="submit" value="수정">
  </form>

  <a href="{% url 'articles:index' %}">목록보기</a>
  
{% endblock content %}
```

```html
# detail.html 
# a태그로 edit으로 갈 수 있도록 함.

<a href="{% url 'articles:edit' article.pk%}" class="btn btn-primary">수정하기</a>
```

## update구현

```python
#urls.py
path('<int:pk>/update/', views.update, name='update'),
#views.py
def update(request,pk):
    article = Article.objects.get(pk=pk)
    article.title = request.POST.get('title')
    article.content = request.POST.get('content')
    article.save()
    return redirect('articles:detail', article.pk)
```

```html
#form의 action 부분에서 url의 articles를 수정해준다.
#edit.html에서 update구현하기

  <form action="{% url 'articles:update' article.pk%}" method="POST">
    {% csrf_token %}
    <label for="title">Title :</label>
    <input type="text" name="title" id="title" value="{{ article.title }}"><br>
    <label for="content">content : </label><br>
    <textarea name="content" id="content" cols="30" rows="5">{{ article.content }}</textarea><br>
    <input type="submit" value="수정">
  </form>
```





