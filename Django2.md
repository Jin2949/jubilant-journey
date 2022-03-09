Django2

HTML 'form' element : 유저에게 input 받기위해 사용한다.

action : 입력 데이터가 전송될 URL지정

method : 입력 데이터 전달 방식 지정
	-get, post 방식이 있다.

GET방식 데이터 전송은 (쿼리 스트링 파라미터)로 한다.

```python
Throw & Catch
<form action = '#' method='#'> #form태그는 action method가 중요
<label for='message'>메세지</label>
<input type='text' name='message' id='message'>
#label for와 input id를 연결시켜서 메세지를 누르면 박스가 깜빡임.
<input type='submit'>
#submit을 주면 url에서 내용이 좀 바뀐다. 

path('catch/', views.catch), 를 urls에 넣어주고
views에 
def catch(request)
	return render(request, 'articles/catch.html')

catch.html에서
extends 'base.html'
block
Catch
endblock
#여기까지 Catch를 만들어주고
Throw에서 form action = '/catch'

def catch(request)
	message = request.Get.get('message')
	context = {'message':message}
    #catch에 들어온 request에서 Get으로 전송된 데이터를 가져올거야 message를 get해줘
	return render(request, 'articles/catch.html', context)
```



#### Variable Routing

```python
urls
path('hello/<str:name>', views.hello), #뒤에 적은거 문자열로 가져와

views
def hello(request, name):
    print(name, '######')
    return render(request, 'articels/hello.html', {'name':name})

#만들고 url에 8000/hello/test를 치게 되면 뒤쪽 test가 name변수에 담겨서 출력하게 된다.
```

```python
urls
path('intro/<str:name>/<int:age>', views.intro),

views
def intro(requst,name,age)
	context = {
        'name':name
        'age':age
    }
	return render(requst, 'articles/intro.html', context)

templates
intro.html
<h1>안녕하세요, {{ name }}, {{ age }} 살 입니다.</h1>
```



```python
새로운 pages앱 추가할경우
urls.py
from pages import views as pages_views	#as사용확인!
	path('pages-index/', pages_views.index),

views.py
def index(request):
    return render(request, 'pages/index.html')

index.html
{% extends 'base.html' %}
{% block content %}
<h1>Pages App의 index.html</h1>
{% endblock content %}

App URL mapping
각 앱에 urls.py를 작성한다. 그리고 안에 URL매핑한다.
원래 프로젝트 URL에는 
from django.contrib import admin
from django.urls import path, include 

urlpatterns = {
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
}
```

#### Naming URL patterns

```python
name ='index'
<a href="{% url 'index' %}">메인 페이지</a>
# 링크에 url을 직접 작성하지 않고, path()함수의 name을 이용해서 지정한다.

url 'articles:greeting' 이름을 정확히 명시해 줄 수 있다.
```


