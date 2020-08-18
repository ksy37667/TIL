# 🔍 Docker Compose로 Django 프로잭트 개발환경 설정하기

* 최근 많은 프로젝트가 Docker를 이용한 컨테이너 기반으로 개발되고 테스트, 배포되고 있습니다.
* Docker Compose를 이용해서 PostgreSQL를 데이터베이스로 사용하는 Django 프로잭트 개발환경을 설정해보겠습니다.

[도커 다운로드](https://www.docker.com/get-started)

## 🔍 requirements.txt
* requirements.txt는 필요한 패키지를 등록해놓는 txt 파일입니다. pip를 통해서 이 파일에 있는 패키지를 설치할 수 있습니다.

* 프로젝트의 최상위 디렉터리에 requirements.txt를 생성합니다. 그리고 아래와 같이 작성합니다.

```
Django
psycopg2
```

* 여기서 psycopg2는 PostgresSQL를 데이터베이스로 사용하기 위해 필요하기 때문에 추가로 등록했습니다.


## 🔍 Dockerfile

* 컨테이너화된 프로젝트에서는 모든 개발이 Docker 컨테이너 안에서 이루어집니다. 그렇게 때문에 개발자의 개발환경이 바뀌더라도 프로젝트에 필요한 패키지를 직접 설치할 필요가 없습니다.

* Docker 컨테이너 안에서 어플리케이션이 돌아갈 수 있게 해주는 Docker 이미지를 만들기 위한 설정 파일입니다. 

* `Dockerfile`은 필요한 최소한의 패키지를 설치하고 동작하기 위한 설정을 담은 파일이고, 이 파일로 이미지를 빌드합니다.
<br><br>
* 다음과 같이 프로젝트의 최상위 디렉터리에 Dockerfile을 생성하고, 다음과 같이 작성합니다.

```docker
FROM python:3
ENV PYTHONUNBUFFERED 1
WORKDIR /web
COPY . .
RUN pip install -r requirements.txt
```

1. 이미지 내 PYTHONUNBUFFERED 환경 변수를 1로 설정

2. 이미지 내 작업 디렉터리를 /web으로 변경

3. 현재 디렉터리의 모든 파일을 이미지 내의 작업 디렉터리로 복사

4. pip를 이용해서 이미지 안에 패키지 설치

<br><br>
* `Dockerfile`를 이용해서 이미지를 빌드하겠습니다.
마지막줄에 성공했다는 메세지와 함께 이미지ID가 출력되었다면 제대로 빌드가 된 것입니다.
```
$ docker build .
```

* 제대로 설치가 되었는지 확인 할 수 있습니다.
```python
D:\cheese>docker run -it f7212e7f5fee
Python 3.8.5 (default, Aug  5 2020, 08:22:02) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
>>> print(django.get_version())
3.1
```


## 🔍 docker-compose.yml
* Docker Compose는 yaml파일로 여러개의 도커 컨테이너의 정의를 작성하여 한번에 많은 컨테이너들을 작동시키고 관리할 수 있는 툴입니다.

* 저희는 Django와 PostgreSQL로 이루어진 프로젝트를 셋업하려고 합니다.

* 위와 동일하게 최상위 디렉터리에 `docker-compose.yml을 생성하고, `web` 서비스로 Django를, `db`를 PostgreSQL로 정의해줍니다.

```yml
version: "3"

services:
  web:
    build: .
    command: python manage.py runserver 0:8000
    ports:
      - "8000:8000"
    volumes:
      - .:/web
    depends_on:
      - db
  db:
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
```

* `web`을 보시면 현재 디렉터리에 있는 `Dockerfile`을 이용해서 이미지를 빌드하고, `python manage.py runserver 0:8000` 커맨드를 실행하게 되어 있습니다.

* `web` 서비스가 실행되기 전에 `db`가 먼저 실행될 수 이도록 `depends-on`을 설정했습니다.


## 🔍 Django 프로젝트 생성
* 터미널에서 `django-admin` 명령어를 실행하여 본인이 원하는 프로젝트 이름으로 생성합니다.

```
docker-compose run web django-admin startproject cheese .
```

* 저같은 경우는 cheese라는 Django 프로젝트가 생성되었습니다.
```
├── Dockerfile
├── docker-compose.yml
├── manage.py
├── cheese
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── requirements.txt
```

## 🔍 Django 프로젝트 실행
* 다음과 같은 명령어를 실행하면 PostgreSQL과 Django가 컨테이너 안에서 구동시킬 수 있습니다.
```
$ docker-compose up
``` 

* 본인의 localhost에 접속하면 장고 초기화면을 볼 수 있습니다.

* 그 후 `setting.py`를 열어서 `DATABASES`를 수정합니다.

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "postgres",
        "USER": "postgres",
        "PASSWORD": "postgres",
        "HOST": "db",
        "PORT": 5432,
    }
}
```

* 그 후 migrate를 해주어야 하는데 저같은 경우는 vs code에서 서버가 전부 실행된 상태로 cmd창을 하나 더 열어서 migrate를 진행했습니다.

```
$ docker-compose exec web python manage.py migrate
```

* 마지막으로 `docker-compose down` 으로 모든 서비스를 종료하고 컨테이너와 네트워크를 삭제할 수 있습니다.


# 참고문서

- [Compose and Django](https://docs.docker.com/compose/django/)
- [Dale Seo님 블로그](https://www.daleseo.com/docker-compose-django/)