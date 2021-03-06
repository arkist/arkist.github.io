---
layout: post
title: "Flask-SQLAlchemy, sqlalchemy-migrate 대신 SQLAlchemy, alembic 써보기"
date: 2014-01-24 03:15:35 +0900
comments: true
categories: python flask sqlalchemy alembic
---

요즘 [Flask](http://flask.pocoo.org/)를 조금씩 보고있다.

[The Flask Mega-Tutorial, Part IV: Database](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database)를 진행하다가, flask-sqlalchemy, sqlalchemy-migrate 대신에 sqlalchemy, alembic 조합으로 갈아타게 된 경험을 정리해볼까 한다.  
서버사이드 웹개발에 대한 (혹은 데이터베이스에 대한?) 이해가 거의 없는 상태에서 정리하는 글이라, 잘못된 부분이 많을수도 있겠다. 

<!-- more -->

페이스북 Flask korea 그룹에서 스터디를 모집하길래 덜컥 신청해놓고 튜토리얼을 버벅이며 조금씩 따라해보고 있다. 난 아주 기초부터 함께하는 초심자용 스터디인줄 알았는데, 그런건 아니었나 보다. Flask 기초지 파이썬 기초가 아니었어.. 

여담은 이만하고 본론으로 들어가자.

----

튜토리얼의 4장 데이터베이스 부분을 보면 DB와 마이그레이션툴로 각각 Flask-SQLAlchemy, sqlalchemy-migrate를 사용하고 있다. 여기에서 고민이 생긴다.

## SQLAlchemy 대신 Flask-SQLAlchemy를 사용한다

Flask에는 [SQLAlchemy](http://www.sqlalchemy.org/)를 위한 확장인 Flask-SQLAlchemy가 존재하며, 튜토리얼에서도 해당 확장을 사용하고 있다. SQLAlchemy를 그대로 쓰는것보다는 설정할것도 줄어들고 코드가 좀 더 단순해져서 다루기가 쉽다.  
하지만 db를 다루는데 Flask의 컨텍스트에 가둬두는게 맞는지 의문이 든다. Flask를 벗어나서 사용을 하려면 결국 SQLAlchemy를 배워야 하는것 아닌가?  
사실 이런 고민은 현재 단계에서 불필요한 고민일 수 있다. 하지만 조금 더 유연한 대처를 위해서 일단 Flask-SQLAlchemy를 걷어내고 생짜 SQLAlchemy를 사용해보기로 했다. 이 부분은 좀 더 공부해보면서 결정하는걸로.

[SQLAlchemy in Flask](http://flask.pocoo.org/docs/patterns/sqlalchemy/)를 참고해보면 Flask에서 SQLAlchemy를 사용하는 4가지 방식을 소개하고 있다. 이곳을 참고하며 케이스에 맞는 선택을 하도록 하자.


## sqlalchemy-migrate를 사용한다

이건 좀 확실히 문제라고 보는게, sqlalchemy-migrate는 이미 (별로 좋지 않은) 구시대의 유물이다. SQLAlchemy 신버전은 지원하지도 않아서 튜토리얼에서도 pip 패키지를 설치할 때 SQLAlchemy의 버전을 구버전으로 강제하고 있다. 이건 별로 좋은 선택이 아니다. 

[sqlalchemy-migrate 프로젝트 페이지](https://code.google.com/p/sqlalchemy-migrate/)에서도 다음과 같이 안내를 하고 있으며,
> If you want to start a new project involving SQLAlchemy and have the need for database schema migrations use [Alembic](https://bitbucket.org/zzzeek/alembic). Alembic is from SQLAlchemy's author and is much better maintained than sqlalchemy-migrate.

[sqlalchemy-migrate의 문제점](http://blog.dahlia.kr/post/8153601295)이라는 글에서도 몇가지 문제점을 지적하고 있다.

뭐, 내 경우에는 그냥 튜토리얼에서 소개된 sqlalchemy-migrate로 구현하는 방식이 별로 우아해보이지 않아서지만.. 

여튼 미래를 위해, alembic으로 갈아타자. 튜토리얼 만든 Miguel 아저씨도 저 튜토리얼 후에 alembic을 사용하기 위한 **Flask-Migrate**라는 확장을 만들어 [Flask-Migrate: Alembic database migration wrapper for Flask](http://blog.miguelgrinberg.com/post/flask-migrate-alembic-database-migration-wrapper-for-flask)에 소개하고 있다. 이 부분은 튜토리얼 문서를 업데이트 해주면 좋을텐데, 책 쓴다고 정신이 없나보다.[^1]

Flask-Migrate는 Flask-SQLAlchemy에 의존성이 있으므로 생짜 SQLAlchemy를 사용하는 내 경우는 이 또한 생짜 alembic을 쓰기로 했다. 웬지 번거로운 선택을 해버린거 같은데..

alembic을 그대로 사용하기 위해서는 설정이 조금 필요하다. alembic의 `env.py`파일의 `target_metadata` 부분에 다음과 같이 선언해주자.

```python alembic/env.py
# target_metadata = mymodel.Base.metadata
import os
import sys
sys.path.append(os.getcwd())
from app import app
from app.database import Base
config.set_main_option(
    'sqlalchemy.url', app.config.get('SQLALCHEMY_DATABASE_URI'))
target_metadata = Base.metadata
```

`SQLALCHEMY_DATABASE_URI` 부분이 핵심이다. flask의 `config.py` 파일에

```python config.py
SQLALCHEMY_DATABASE_URI = 'postgresql://localhost/microblog'
```

와 같이 선언해두고 SQLAlchemy와 alembic에서 공통으로 참조할 수 있도록 했다. 
SQLAlchemy 설정에서도  create_engine 부분을 다음과 같이 선언해 두었다.

```python database.py
engine = create_engine(
    app.config.get('SQLALCHEMY_DATABASE_URI'), convert_unicode=True)
```


## 정리 
일단 여기까지 해두었더니 기본적인 셋팅은 완료된 것 같다. SQLAlchemy/alembic 조합으로 셋팅을 마치고 Flask 튜토리얼을 계속 진행하고 있다.

혹시 내가 놓치고 있거나 잘못된 부분은 피드백 부탁드린다. (__) 


[^1]: 튜토리얼 내용 엮어서 책으로 내는 것 같더라. <http://blog.miguelgrinberg.com/post/flask-book-and-pycon-update>



