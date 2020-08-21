# 🔍 User Model 이란?
* 우선 Django User Model을 커스터마이징 하기 전에 User Model에 대해서 간단하게 설명하자면 Django에 기본적으로 정의되어 있는 User Model 입니다.

* 이 모델을 사용해도 좋지만 서비스를 개발할 땐 기본적으로 Django에서 제공햊부는 모델보다 더 다양한 기능과 정보들을 필요로 할 때가 많기 때문에 서비스를 개발 할 때 User Model을 그대로 사용하는 경우는 거의 없다.


# 🔍 그래서 Django User Model을 커스터마이징 한다.
* 기본 User Model을 확장(커스터마이징)하는 방법은 여러가지가 있다.
    * Proxy Model
    * User model 과 one-to-one 연결
    * AbstractUser 모델 상속한 사용자 정의 User 모델 사용하기
    * AbstractBaseUser 모델 상속한 사용자 정의 User 모델 사용하기

<br><br>
## Proxy model
* 프록시 모델은 새 테이블을 추가하거나 스키마의 변경 없이 단순히 상속만 하는 방식이다. 정렬순서나 기존 모델의 동작을 변경하는데에 필요한 메소드만 추가하기 위해 사용된다. Django의 기존 User Model에 추가적인 사용자 정보를 저장할 필요가 없을 때 사용하는 방법이다.

```python
from django.contrib.auth.models import User
from .managers import PersonManager

class Person(User):
    objects = PersonManager()

    class Meta:
        proxy = True
        ordering = ('first_name', )

    ...
```
* 코드와 같이 `first_name`을 기준으로 정렬하는 방식으로 변경할 수 있다.
* 여러가지 메소드를 추가할 수 있고 `User.objects.all()`은 `Person.objects.all()`과 같은 쿼리로 동작한다.(스키마의 변경이 없기 때문)

<br><br>

## User model 과 ont-to-ont 연결

* 기존 User 모델과 OneToOneField 로 일대일 관계를 맺는 Django 모델을 추가해서 사용자에 관한 정보를 저장하는 것이다.
* Django의 인증 시스템을 그대로 활용면서 로그인, 권한 부여 등과 상관이 없는 사용자 정보 필드를 저장할 때 사용하는 기법이다.

```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    user_pk = models.IntegerField(blank=True)
    nickname = models.CharField(max_length=200, blank=True)
    point = models.IntegerField(default=0)
    phone = models.CharField(max_length=200, blank=True)


@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance, user_pk=instance.id)
    
@receiver(post_save, sender=User)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()
```
* 여기서 receiver 라는 생소한 문법이 보일텐데 여기에선 'User Model instance가 생성되면 Profile 모델 인스턴스가 함께 생성된다' 정도로 이해하면 된다.
<br><br>

## AbstractBaseUser 모델을 상속한 사용자 정의 User 모델

* `AbstractBaseUser`모델을 상속한 User 모델을 만들고 `Settings.py에 참조를 수정해서 사용한다.
* 기본적인 User model 에서는 username을 id로 사용하지만 email을 id로 사용한다거나, 다른 다양한 정보들을 추가하고 싶을 때 사용한다.

```python
class User(AbstractBaseUser, PermissionsMixin):
    objects = UserManager()

    email = models.EmailField(verbose_name = "email id", max_length = 64, unique = True)
    username = models.CharField(max_length=30)
    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = []
    date_joined = models.DateTimeField(_('date joined'), default=timezone.now)

    EMAIL_FIELD = 'email'
    USERNAME_FIELD = 'email'

    def __str__(self):
        return self.username

    def get_short_name(self):
        return self.email

    class Meta:
        verbose_name = _('user')
        verbose_name_plural = _('users')

```python
class UserManager(BaseUserManager):
    use_in_migrations = True # 선택적으로 관리자를 마이그레이션으로 직렬화한다.
                             # True로 설정된 경우 관리자가 마이그레이션으로 직렬화되며...?

    # 유저 생성
    # 파라미터로 전달받은 값들을 user 객체로 db에 저장한다
    # nomalize 중복 최소화를 위한 정규화?
    def _create_user(self, email, password, **extra_fields):
        if not email:
            raise ValueError('이메일이 없습니다.')

        eamil = self.normalize_email(email)
        user = self.model(email = email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    # 일반 유저 생성
    # 일반 사용자를 생성한다. is_staff, is_superuser = False ==> 일반유저라는 뜻
    def create_user(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', False)
        extra_fields.setdefault('is_superuser', False)
        return self._create_user(email, password, **extra_fields)


    def create_superuser(self, email, password, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('관리자는 is_staff가 True여야함')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('관리자는 is_superuser가 True여야함')

        return self._create_user(email, password, **extra_fields)
```

* 커스텀 유저 모델을 사용하기 위해서는 `BaseUserManager`와 `AbstractBaseUser`를 구현해야 한다. `BaseUserManager` 클래스는 User를 생성할때 사용하는 클래스이고 `AbstractBaseUser` 클래스는 상속받아 생성하는 클래스이다.

* UserManager
    * create_user: 일반 유저 생성
        * 일반적인 유저를 생성한다.
        `'is_staff', False` 와
        `'is_superuser', False` 를 써서 일반 사용자로 설정하고 _create_user로 보낸다.

    * create_superuser: 관리자생성
        * create_user와 반대로 두 값에 True를 주고 두 값이 False라면 에러 메세지를 출력한다.

    * _create_user: 유저 생성
        * `create_user`와 `create_superuser` 에서 전달받은 값들을 user객체에 저장하고 DB에 저장한다.

* 그 후 `settings.py`에 `AUTH_USER_MODEL = '[APPNAME].User'`를 설정해줘야 하는데 내가 가장 많이하는 실수이다.
<br><br>

## AbstractUser 모델 상속한 사용자 정의 User 모델 사용

* User Model을 그대로 가져다 쓰는 대신 필드만 재정의 할 때 사용한다

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    bio = models.TextField(max_length=500, blank=True)
    location = models.CharField(max_length=30, blank=True)
    birth_date = models.DateField(null=True, blank=True)
```

* 위와 똑같이  `settings.py`에 `AUTH_USER_MODEL = '[APPNAME].User'`를 설정해줘야한다.