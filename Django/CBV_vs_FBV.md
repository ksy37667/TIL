# 🔫 함수형 뷰(FBV)
- 개발속도가 빠를 순 있지만 코드가 길어지면 로직이 복잡해지는 단점이 있다.


# 🔫 클래스형 뷰(CBV)
- 다중상속과 같은 객체지향 기법을 활용해 generic view, mixin class 등을 사용해 코드의 재사용과 개발 생산성을 높여준다.
- HTTP 메소드(POST, GET 등)에 따른 처리 코드를 작성할 때 if 대신에 메소드 명으로 코드의 구조가 깔끔하다.

# 클래스 기반 뷰를 이용할 때의 가이드라인
- 뷰 코드의 양은 적을수록 좋다.
- 뷰 안에서 같은 코드가 반복되지 않도록 한다.
- 뷰는 프레젠테이션 로직에서 관리하고 비즈니스 로직은 모델에서 처리한다. 특별한 경우에는 폼에서 처리한다.
- 403, 404, 500 에러 핸들링에는 CBV보단 FBV를 이용한다.
- 믹스인은 간단명료해야 한다.

# 예제코드
```python
# views.py

from django.views.generic.edit import FormView 
from .models import User
class RegisterView(FormView):
    model = User
    template_name = 'register.html'
    success_url = '/'

    def form_valid(self, form):
        # 커스텀 로직이 위치
        return super().form_valid(form)
```

이렇게만 해주면 가장 기본적인 클래스형 뷰가 완성된다.


# reference
https://docs.djangoproject.com/en/3.0/topics/class-based-views/generic-display/