django3

Namespace : 설정해서 각 페이지의 index가 이름이 같더라도 표시할 수 있도록 해준다.

Static files: 정적파일이라고 하며, 응답할 때 별도 처리 없이 그대로 보여주면 되는 파일

- settings.py 에 INSTALLED_APPS에 포함되어 있다.
- settings.py 에서 STATIC_URL을 정의
- static 경로는 app/static/ 이다. <-> template는 app/template

STATIC_ROOT : 배포환경에서 사용한다. (경로를 모아주기 위해서 사용한다.)

static tag를 가져오기 위해서는 load와 static이 필요하다(빌트인이 아니기떄문에)

강의

- articles-static 에 이미지를 넣음
- index.html에 {% load static %} 를 넣어줌.
- block에 이미지 삽입, {img~="{% static 'sample-img.jpg' } %}" alt = "sample image"> 