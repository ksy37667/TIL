## 🔍Django에서 외래키(foreign key)에 해당하는 모델 값 직렬화 하는법

* 장고 모델을 Serializing해서 JSON 형태로 프론트앤트에 뿌려주는 API 만들었다.

```python
# serializer.py
class BoardSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Board
        fields = '__all__'
```

```python
# view.py

class BoardViewSet(viewsets.ModelViewSet):
    queryset = Board.objects.all()
    serializer_class = BoardSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
```

* 그리고 JSON 데이터를 보면 이러한 형태이다.
```JSON
{
    "id": 11,
    "title": "제목",
    "content": "내용",
    "author": 1,
    "created_at": "2020-08-29T12:09:32.628094+09:00",
    "updated_at": "2020-08-29T12:09:32.628110+09:00"
},
```


내가 원하는건 `foreinkey(id값)`이 아닌 그 외래키에 해당하는 `username`을 가지고 오고 싶었다.
처음에는 `views.py`에서 보내는 response의 값을 어떻게든 제어하면 되지 않을까 싶어서 수 시간의 삽질을 하다가 실패했고, 결국 다음날이 되서야 방법을 찾을 수 있었다.

```python
class BoardSerializer(serializers.ModelSerializer):
    username = serializers.ReadOnlyField(source='author.username')

    # print(author)
    class Meta:
        model = Board
        # fields = '__all__'
        fields = ('id','title', 'content', 'username', 'created_at', 'updated_at')
```
* 다음과 같이 BoardSerializer에 `author.username`을 username에 제공해줘서 직렬화 할 때 원하는 속성을 추가해주는 방식으로 해결했다.
<br>
<br>

## 참고자료
[Stack Over Flow](https://stackoverflow.com/questions/17280007/retrieving-a-foreign-key-value-with-django-rest-framework-serializers)