## alsj this is test post

asdfjilr
---
>
jkls`1dgrs`jfie
---
# 제목
## 부제목
내용 작성
## 부제목2
우아아아
# 제목2

---
선 orm을 사용하려면 단 두가지가 필요하다.

- model class : db 매핑 클래스
- sqlalchemy 공식 문서를 보고 orm api를 익힐 수 있는 시간과 인내심
    
    : 블로그나 stack overflow를 참고할 수 있지만, 대부분 sqlalchemy 2.0이 아닌 구버전(1.x) 스타일의 코드이거나 딱 원하는 동작을 하는 내용을 찾기가 어려워 차라리 공식문서를 보는게 시간이 걸리더라도 개인적으로 나았다.
    
    공식문서 : [ORM Quick Start — SQLAlchemy 1.4 Documentation](https://docs.sqlalchemy.org/en/14/orm/quickstart.html)
    

이 중 이번 포스팅에선 매핑 클래스인 model class를 만들어보자.

### model class

db의 데이터 모델(테이블, 뷰 등)을 클래스로 표현한, orm을 사용할 때 각 데이터 모델을 클래스로 매핑할 클래스이다.

sqlalchemy 라이브러리