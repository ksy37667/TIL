# 🔍Django Rest Framework - Serializer
* `Serializer`는 queryset이나 model instance 같은 복잡한 데이터를 JSON으로 반환하게 해준다.
* 어떻게보면 Django의 Form과 유사할 수 있는데 Form은 html을 직접 생성하고 `Serializer`는 JSON을 생성한다.


# 🔍실습
* 우선 실습에 필요한 모델을 만들었다.
```python
from django.fb import models

class Post(models.Model):
    title = models.CharField(max_length=128)
    content = models.TestField()
    postuser = models.Foreignkey('postuser.Postuser', on_ondelete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    update_at = models.DateTimeField(auto_now=True)
```
<br>

* 그리고 Serializer를 만든다.
```python
# serializers.py

from rest_framework.serializers import ModelSerializer
from .models import Post

class PostSerializer(ModelSerializer):
    class Meta:
        model = Post
        fields = '__all__'
```
<br>
* view 로 불러와서 처리하는 방법이 여러가지가 있는데 여기서는 가장 간단한 `ViewSet`을 사용한다.

* `ViewSet`을 사용하면 특정 레코드의 CRUD 및 Retrieve를 하나의 클래스로 구현할 수 있다.
    * viewsets.ReadOnlyModelViewSet: 목록조회, 특정 레코드만 조회
    * viewsets.ModelViewSet: 목록조회, 특정 레코드 CRUD
<br><br>
* queryset과 serializer_class를 지정해준다. view에서 Django의 `Form`을 사용할 때 `template_name` 이나 `model` 을 지정해준것과 비슷하다고 생각하면 된다.

```python
# views.py

from .models import Post
from .serializers import PostSerializer
from rest_framework import viewsets

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

* 그 후 `urls.py` 에서 라우팅을 해주는데 기존에 사용하던 방식과는 조금 다르다. `Router`를 사용하는 방식이다.

```python
# urls.py 

from django.contrib import admin
from django.urls import path, include
from quickstart.views import PostViewSet
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register('post',PostViewSet) #url로 사용할 이름, 사용할 ViewSet

urlpatterns = [
    path('',include(router.urls)),
    path('admin/', admin.site.urls),
]
```

# 🔍결과 화면
* `http://127.0.0.1:8000/` 

![](https://github.com/ksy37667/TIL/blob/master/DRF/Serializer/img/drf1.PNG)

* `http://127.0.0.1:8000/post`

![](https://github.com/ksy37667/TIL/blob/master/DRF/Serializer/img/drf1.PNG)

* `http://127.0.0.1:8000/post/<int:pk>/`

![](https://github.com/ksy37667/TIL/blob/master/DRF/Serializer/img/drf1.PNG)